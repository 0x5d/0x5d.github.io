---
title: "Rust Adventures: External Sort, Chapter 1"
date: 2024-11-16T20:19:47-05:00
lastmod: 2024-11-16T20:19:47-05:00
publishDate: 2024-11-16T20:19:47-05:00
tags: ["rust", "algorithms"]
ShowReadingTime: true
ShowToc: true
---

I've been learning Rust in my free time for the last 6 months. I read The Book and I'm in the middle of reading Rust for Rustaceans ([my notes so far](https://blog.nerv.industries/posts/rust-for-rustaceans/)), but after doing a couple of very small exercises and projects I wanted to do something more _rusty_.

So on Sundays, in between espressos and with very good company, I started writing an external sort.

This is the 1st post in a series, where I cover the code that generates the data to be sorted. Stay tuned for the next one!

> All the code & commands I'll post here are running on my Macbook Pro M1 (32GiB, 1TB SSD). This is just for fun.
> The code is hosted in [GitHub](https://github.com/0x5d/ext-sort).

# The problem

[External sorting](https://en.wikipedia.org/wiki/External_sorting) algorithms are used when the data that must be sorted can't fit in memory. For example, my Mac has 32GiB of RAM. How can I sort a larger file?

So the basic idea is:
- you get a big file (bigger than memory), 
- you split it into smaller chunks that fit in memory,
- you sort those chunks and write them to disk,
- you merge the sorted chunks and write the output, sorted.

I will go deeper into details as we go into my implementation.

# The Solution

**Goals**

- Make this thing **fast** af.
- Drink good espresso.
- Go into rabbit holes, learn about hardware, the OS, and performance.

**Non-goals**

- To write a production-ready Rust crate. This is just a fun exercise.

## Sorting lots of random data

Hold on, to sort lots of random data we need to start by -

## Generating lots of random data

> Soundtrack: [Turnstile - Generator](https://www.youtube.com/watch?v=MDRUwlqa6N0)

Ok, so the first thing we need is a way to generate random data. For this project, we'll consider a 4KiB (AKA a "page") ASCII string a _unit_ or _block_ of data. This will be the resolution we'll be working at. The source file will be composed of many 4KiB strings with no gaps or delimiters.

### Writing\* lots of random data fast\*

One of the **Goals** is to make this thing fast. So let's think about that for a second - what does _fast_ mean in this context? What determines the higher bound for time in this algorithm? It requires 3 things:

- Memory
- CPU
- IO (SSD)

If the input grows, we _could_ add more CPU and memory, but IO speeds would remain constant - NVMe SSDs have a physical write speed limit, so we'll probably be limited by it. Let's see what that looks like.

### How "fast" is "slow"?

There are some unofficial sources which have published benchmarks for Apple MacBook Pro SSDs, but I wanna check by myself.

To do so, I'll use [`fio`](https://formulae.brew.sh/formula/fio) (the Flexible IO CLI, but nobody I know calls it that), a disk benchmark tool. It has a lot of knobs (it's flexible!), and I encourage you to run `man fio` to check them out. However, we're only interested in knowing the write rate (mebibytes per second), so this is how we'll run it:

**NOTE**: The following command will create a 100GiB file in the directory where its run. Think before you run it!

```sh
fio --name=write        \
    --ioengine=posixaio \
    --rw=write          \
    --bs=1m             \
    --numjobs=1         \
    --size=100g         \
    --iodepth=8         \
    --end_fsync=1       \
    --direct=1
```
**Flags explanation**
- `--name=write`: Just a name for the fio "job". It will use it for the output and the file it creates.
- `--ioengine=posixaio`: The IO engine for MacOS, POSIX Asynchronous IO.
- `--rw=write`: We're only interested in measuring performance with sequential writes.
- `--bs=1m`: A block size of 1MiB. If it was smaller, `fio` would need to make lots of repeated `write` syscalls, adding a lot of noise to the test (since they're expensive - more on this later).
- `--numjobs=1`: The number of parallel jobs performing IO (writing, in this case). Since we're doing sequential IO, adding more jobs won't do any good - they'll contend for file access.
- `--size=100g`: A big file!
- `--iodepth=16`: The queue of in-flight IO operations against the file. This is determined by a combination of the OS, the `fio` engine (`posixaio` in this case), and other factors. `fio`'s output shows a distribution of the queue size, which helps determine the maximum value here. If a smaller-than-possible value is used, the benchmark might spend lots of time waiting for IO operations to be _acknowledged_. A value of 1 would make it behave synchronously.
- `--end_fsync=1`: Only consider the test done when the file is flushed to disk.
- `--direct=1`: Use direct IO (as opposed to buffered IO). In MacOS, this is controlled by `fcntl`'s `F_NOCACHE` flag (see `man fcntl`). In Linux, this is equivalent to setting the `O_DIRECT` flag when opening a file. 

I will omit most of the output since right now we're only looking for the average write rate, expressed by the `bw` (bandwidth) output field, which in this case is ~5736MiB/s (I averaged it over multiple runs). If you're curious about what the rest of the output means, the [official docs](https://fio.readthedocs.io/en/latest/fio_doc.html#interpreting-the-output) have a great explanation.

So now that we know the practical upper limit that we can hit with the generator, we can move on to it.

### Considerations

#### File size

Simple one. We'll allow the file size to be configurable. However, since we're treating a 4KiB block as our unit, the file size will be 4KiB-aligned (that is, we must check that its size is a multiple of 4KiB).

#### Memory limit

The reason for using an external sorting algorithm is not having enough memory to hold the data being sorted. Therefore, we need to take care not to go over the available memory. We'll allow it to be configurable via a flag.

#### Parallelism

Generating random ASCII strings will be slow, so we'll benefit from using multiple threads. We'll use [Tokio](https://tokio.rs/) for that.

#### Saturation

The SSD must be saturated at all times, so that we don't add artificial bottlenecks.

### The code

The strategy I followed is as follows:

- Create the file. In MacOS, the file is opened with Direct IO by default. In Linux, you'll need to be careful to open it with the `O_DIRECT` flag set.
- Spawn a "writer" thread. It will hold a handle to the open file and receive strings over a channel, which it will write to the file. Since the strings are random, it won't do any ordering, writing them in a FIFO fashion.
- Start threads to generate the random strings, sending them over the channel. This is where memory is allocated, so we must have a way to enforce the configured limit.

#### Sidequest: limiting memory

{{< gist 0x5d d10c742fca7b23fea5cea5a5e9885e67 bucket.rs >}}

To make it easier to enforce the memory limit, I wrote a simple rate-limiting [token bucket](https://en.wikipedia.org/wiki/Token_bucket).

It's initialized with a `capacity`, and threads can `take` and `put` tokens back. If a thread tries to take a token when the bucket doesn't have any, it will sleep until a token is put back by another thread.

The implementation isn't perfect (e.g. it doesn't prevent putting tokens over the initial capacity), but it's enough.

#### Back to the main thing

Ok, back to random string generation. Here's the code in full. I'll go over each part.

{{< gist 0x5d d10c742fca7b23fea5cea5a5e9885e67 generate.rs >}}

First, the signature:
```rust
pub async fn generate_data(filepath: &str, size_bytes: usize, max_mem: usize) -> io::Result<()>
```

`generate_data` is an async function. It takes a `filepath` where the file will be created, the file's size, `size_bytes`, and the program's memory limit: `max_mem`. It returns an `io::Result` to communicate that it can fail.

```rust
if (max_mem as usize) < crate::BLOCK_SIZE {
    return io::Result::Err(Error::new(
        io::ErrorKind::Other,
        format!(
            "Max allowed memory must be larger than {}B",
            crate::BLOCK_SIZE
        ),
    ));
}
```
The max memory must be at least 1 block (4096B) - otherwise we won't be able to do anything! Here's a good place to make it clear that `max_mem` doesn't account for the memory things on the stack - pointers, numbers, etc.

```rust
let file = File::create(filepath)?;
let num_cores = std::thread::available_parallelism()?.get();
let mem_per_core = max_mem / num_cores;
let b = Arc::new(bucket::Bucket::new(num_cores as i32));
let mut set = JoinSet::new();
let (tx, rx) = mpsc::channel();
```
Next up, the values used throughout the function at are initialized:

- `file`: The file into which we'll write the random strings.
- `num_cores`: The number of cores available in the CPU.
- `mem_per_core`: The maximum amount of memory (in bytes) that each thread will be able to use. 
- `b`: The `Bucket` that will serve as the memory-limiting mechanism. It's wrapped in an `Arc` so that it can be "shared" across threads.
- `set`: A Tokio `JoinSet`, which we'll use to keep track of the threads, and to join them when work is finished.
- `tx, rx`: The send and receive ends of a channel, which we'll use to transmit strings from the "generator" threads to the "writer" thread.

About that...

```rust
let writer_handle = tokio::spawn(writer(file, b.clone(), rx, num_cores));
// ...
async fn writer(
    mut file: File,
    b: Arc<Bucket>,
    rx: Receiver<String>,
) -> io::Result<()> {
    loop {
        let s = rx.recv().map_err(|e| {
            Error::new(
                ErrorKind::Other,
                format!("Could not receive from channel: {e}"),
            )
        })?;
        if s == POISON_PILL {
            return Ok(());
        }
        b.put();
        file.write_all(s.as_bytes())?;
    }
}
```
The `writer` thread writes the strings it receives over `rx`, and `put`s back a token into the bucket, so any waiting generator threads are unblocked. When generation is finished, a [`POISON_PILL`](https://aws.amazon.com/message-queue/features/#:~:text=Poison%20pills%20are%20special%20messages,in%20a%20client%2Fserver%20model.) value is sent, signaling it to stop.

Now, onto the main loop in `generate_data`:
```rust
let mut remaining = size_bytes;
while remaining > 0 {
    let len = min(remaining, mem_per_core);
    remaining -= len;
    let tx = tx.clone();
    b.take();
    set.spawn_blocking(move || generate(tx, len));
}
```
Here, a new generator task is spawned for every _chunk_ of string that must be generated, until there are no more `remaining` bytes. The string will be at most `mem_per_core` long. A token is taken from the bucket `b`, to prevent the program from going over the `max_mem`. `generate` is called, which generates a random string and sends it over `tx` so that the writer thread writes it to the file.

```rust
while let Some(res) = set.join_next().await {
    let _ = res??;
}
tx.send(String::from(POISON_PILL))
    .map_err(|e| io::Error::new(ErrorKind::Other, e))?;

writer_handle.await?
```

Finally, we join all the generator tasks, send the `POISON_PILL` message to the writer, and join it.

### The moment of truth

According to the write rate output by `fio` (5736MiB/s), _just_ writing 100GiB file should take around ~17.9 seconds. We should expect some overhead from the random string generation, though.

I created a [small CLI with the `clap` crate](https://github.com/0x5d/ext-sort/blob/main/src/main.rs#L33-L35) to make it easier to generate the file. Wrapping the call with `time` gives us:

```sh
time ./target/release/ext-sort gen --file data.txt --size 107374182400
./target/release/ext-sort gen --file data.txt --size 107374182400  176.07s user 21.10s system 924% cpu 21.323 total
```

That's just over 3 seconds more than fio, which is a little bit of overhead. If you see any chances for reducing it, please let me know over at [Twitter](https://x.com/_0x5d) or [Bluesky](https://bsky.app/profile/0x5d.bsky.social)!

## The real friends were the walls we crashed into along the way

> Soundtrack: [Turnstile - Endless](https://youtu.be/ccLAgkz2eGI?si=iGSnoGf1BH429mi8)
>
> _When I hit a wall I gotta BREAK. IN._

Here are some things I ran into.

### Always use `--profile release`

... before you test performance. `cargo build` on its own doesn't apply all possible optimizations, to make compilation faster. The result is abysmal:

```sh
cargo build
time ./target/debug/ext-sort gen --file data.txt --size 107374182400
./target/debug/ext-sort gen --file data.txt --size 107374182400  1825.21s user 28.37s system 963% cpu 3:12.48 total
```

3:12.48! That's over 9x more than with the release build.

### JoinSet::spawn_blocking vs JoinSet::spawn

Using `spawn_blocking` yields slightly faster (~600ms for 100GiB files) run times than `spawn` in this case. This caught me off-guard at first, but upon inspection, this probably happens because in `generate`, `SmallRng::from_entropy()` issues a system call to `getentropy` (`getrandom` in Linux).

### File::write vs File::write_all

The `write` docstring says it clearly:

> This function will attempt to write the entire contents of `buf`, but the entire write might not succeed [...]

So if you wanna make sure the whole buffer is written to the file, call `write_all` (or do what `write_all` does, which is to call `write` as many times as needed).
