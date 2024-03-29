---
title: "Sobre escribir RFCs"
date: 2023-02-18T15:11:47-05:00
lastmod: 2023-03-12T15:33:47-05:00
publishDate: 2023-03-12T15:33:47-05:00
etiquetas: ["diseño"]

ShowReadingTime: true
---

Aquí voy a escribir el artículo que me gustaría haber leído cuando empecé a escribir RFCs en mi trabajo. Empezaré dando algunas razones para escribir RFCs, y luego hablaré sobre mi proceso _actual_ y una [plantilla] que puedes usar.

Este artículo no es una especificación formal de cómo redactar RFCs. Más bien se trata un sistema flexible que me ha resultado útil en los últimos 2 o 3 años. Me tomó un rato entender el valor de los RFCs, mas ahora los considero una herramienta muy valiosa. Si ya eres un experto en RFCs, tal vez encuentres aquí una perspectiva diferente, y si tienes algún consejo para mí, ¡por favor cuéntamelo! Si nunca has escrito un RFC, espero que al final de este artículo sientas que vale la pena el esfuerzo y les des una oportunidad pronto. 

**Espera un momento - qué es un RFC?**
RFC significa "Request for Comments", o "Petición de Comentarios", en español. Es un documento estructurado escrito en lenguaje natural donde se explica una propuesta. La mayoría de las veces se trata de propuesta técnica, pero un RFC bien podría ser usado para proponer otros cambios, como por ejemplo en un proceso de tu organización.

## ¿Por qué?

He identificado 3 beneficios principales de los RFCs. Cada uno está relacionado con una fase general del proceso de toma de decisiones: el _antes_, el _durante_, y el _después_. Para resumir, me gusta que incentivan un **análisis previo** del cambio y su impacto, fomentan la **colaboración asíncrona** y las discusiones serias y pausadas, y sirven como **referencia** para entender cómo y por qué la decisión fue tomada.

En una organización saludable, todo esto debería conducir a que gastes menos tiempo que si te metieras de lleno a escribir código desde el principio. Es contraintuitivo, pero después de dedicar tiempo escribiendo un buen RFC, el camino a seguir debería ser más claro y fácil de andar.

### El antes: Análisis previo

La escritura de un RFC comienza con una exploración meticulosa de los espacios del problema y la solución (es decir, la definición del problema y el conjunto de posibles soluciones). El peroblema puede haber sido descubierto por tí (por ejemplo, que el rendimiento de una consulta de base de datos ejecutada frecuentemente se haya degradado), o te pudo haber sido asignado para que lo investigues (como un nuevo caso de uso que el producto en el cual trabajas debe tener en cuenta). Sea cual sea el escenario, el problema en sí, así como los requisitos que surjan de éste deben ser desglosados.

Esta descripción del problema y los requisitos (tanto funcionales como no funcionales) serán de ayuda al momento de analizar las diferentes alternativas para resolverlo. Éstos restringen el espacio de la solución, llevándote hacia la solución óptima (aún cuando esta no sea la "ideal").

Luego, es esencial identificar los coautores, colaboradores e interesados, así como tu audiencia.
- Los coautores trabajarán junto a ti en la elaboración del RFC, haciéndose cargo de partes de él o compartiendo toda la responsabilidad contigo.
- Los colaboradores son personas que pueden ayudarte discutiendo ideas contigo o guiándote. Por ejemplo, una compañera responsable de un subsistema con el que tu código deberá interactuar, o un miembro del equipo de producto que pueda ayudarte a entender ciertos casos de uso.
At this point, it's essential to identify co-authors, collaborators, stakeholders, and your audience.
- Un interesado es cualquier persona que se pueda ver impactada por la potencial implementación de tu RFC. Es un grupo naturalmente "abierto" que puede incluir colegas que deberán re-priorizar su trabajo, sus managers o incluso clientes que están a la espera de una nueva funcionalidad.
- Tu audiencia es quien sea que lea tu RFC. Es decir, cualquiera cuyos comentarios u opiniones quieras recibir. Debes identificarla tan pronto como sea posible, ya que definirá casi todos los aspectos de tu RFC: tu estilo al escribirlo (ej. serio o informal), el tipo y la cantidad de material auxiliar que debas incluir (como diagramas, pedazos de código, prototipos de los cambios en la UI, etc.). Si no logras establecer quiénes forman tu audiencia, será más difícil obtener sugerencias de buena calidad. Una señal común de que debes revaluar quiénes la conforman es si empiezas a recibir comentarios indicando que debes dar más contexto en alguna parte, o que debes explicar algo mejor. En estos casos tu reacción no debe ser nunca excluir a las personas haciendo estos comentarios, sino identificar su posición y adaptar tu RFC para que sea más digerible para ellos.

Identificar estos cuatro grupos ayuda muchísimo a reducir tanto la magnitud del riesgo como los diferentes tipos de riesgo asociados con tu propuesta.

La primera dimensión de riesgo es la de utilización de recursos. Después de haber trabajado concienzudamente en tu RFC, si es aprobado, entenderás mejor **qué** se necesita hacer, **quiénes** lo harán, y **cuánto tiempo** tomará. Por otro lado, si esrechazado, no habrás desperdiciado tu valioso tiempo trabajando en un proyecto potencialmente complicado sólo para darte cuenta que no iba a funcionar. Se podría decir que nuestro trabajo como programadores no es escribir tanto código como sea posible, sino minimizar la cantidad de código escrito.

La segunda dimensión es la gestión del cambio. Tu propuesta inevitablemente afectará el sistema que modifica. Pensar profundamente en el problema y su solución te ayudará a ver cómo la implementación de tu RFC impactará otros componentes y usuarios, facilitando la ideación de una estrategia de migración.

La tercera es el impacto en las personas. Si te encuentras escribiendo un RFC, lo más probable es que el problema al que te enfrentas no sea trivial. Como lo dije antes, es posible que su implementación requiera cambios en otros sistemas, así como un plan para desplegarlos (o un plan de migración).Esto significa que tendrás que obtener la aprobación de las personas responsables de dichos sistemas. Los RFCs suponen un excelente espacio para colaborar con ellas hasta que lleguen a un consenso. Si tienen alguna duda, puedes contextualizar mejor tu propuesta para hacerla más clara. Si encuentran un defecto en la solución, puedes tratar de encontrar una alternativa mejor.
Cuando todas las personas relevantes hayan aprobado tu RFC, entenderán claramente su rol (o el rol de su equipo) en su implementación, y podrán planear su trabajo según lo requieran.

### Durante: Colaboración (asíncrona)

Algunos RFCs son simples: todos conocen el problema, la solución resulta ser de alcance limitado y el plan de ejecución es claro. Otros, sin embargo, son complejos y necesitan que varias personas colaboren escribiéndolo y revisándolo. En esas ocasiones, reunirse con todos en una videollamada suele ser un método ineficiente de recopilar opiniones e ideas. La forma óptima de escribir y revisar RFCs es con herramientas que fomenten la comunicación asíncrona.

Si estás escribiendo el documento junto a alguien más, pueden contribuir a él en los momentos del día en que sean más productivos. De hecho, trabajar en RFCs suele ser un ciclo de escribir, alejarse para pensar y examinar opciones, dibujar diagramas, leer código y otros RFCs y esbozar soluciones. Por lo tanto, sincronizar múltiples personas para que siempre estén en las mismas fases es casi imposible.

Para quienes revisen el RFC, un patrón asíncrono de comunicación les da tiempo de leer todo el documento, tomar notas y pensar en sugerencias y preguntas valiosas. Esta es una alternativa mejor a obligar a alguien a formular opiniones y preguntas en un instante. También, como autor, te permite reflexionar sin afán sobre los comentarios hasta el momento, para evitar malos entendidos y el spam. Si quienes están participando en la sección de comentarios del documento perciben que hay demasiados comentarios cortos (como si fuera un chat en tiempo real), probablemente la ignoren totalmente.

Asimismo, los RFCs desacoplan la definición de la ejecución. Los autores no tienen que ser necesariamente quienes implementen la solucion descrita en el documento, reduciendo el [_bus factor_](https://es.wikipedia.org/wiki/Bus_factor) en el equipo. Por ejemplo, piensa en un escenario donde no se escribió ningún RFC, y la persona a cargo de solucionar el problema cambia de equipo o se va de la empresa. Normalmente, leer (en algunos casos, _descifrar_) el código que dejaron toma mucho más tiempo.

### Después: Referencia

Luego de haber implementado el RFC, éste seguirá siendo un recurso invaluable, ya que alguien que tenga preguntas sobre por qué las cosas se hicieron de cierta forma podrá referirse a él. ¡A veces, incluso la respuesta a por qué fueron hechas las cosas, independientemente del cómo, no es para nada clara tampoco!

En una industria tan dinámica (para ponerlo en términos suaves), seguramente en el transcurso de algunos meses algunos de tus colegas cambiarán de equipo o se irán de tu empresa. ¡Y eventualmente tú también lo harás! Entrar a un equipo que tiene una colección de RFCs bien escritos describiendo las partes más importantes del sistema es un alivio.

Otro efecto secundario menor (pero importante) de todo esto es facilitar que las personas que entren a la empresa empaticen con las personas que llevan más tiempo. Es fácil juzgar a alguien cuando encuentras una parte del código que contribuyeron y que no entiendes o que no cumple con los estándares que esperarías. El código "feo" no debería evaluarse sin contexto, y los RFCs ayudan en esos casos. Por ejemplo, si los requisitos fueron documentados, podrían decir mucho del trasfondo en el cual el código fue escrito, así como de las circunstancias que rigieron la implementación

## ¿Cuándo?

Con el tiempo desarrollarás un instinto para saber cuándo debes escribir RFCs. Sin embargo, una buena regla general es revisar si alguno de los beneficios que mencioné antes (análisis previo, colaboración, referencia) son o serán valiosos para ti. Si al menos uno te parece buena idea, el tiempo que dediques a escribir un RFC valdrá la pena. Es común pensar que los RFCs no son útiles para equipos pequeños, donde todos están en sintonía y saben qué hay que hacer, pero te sorprendería ver qué tan a menudo nuestras ideas sobre cómo funcionan las cosas (y sobre cómo pueden cambiar) resultan estar erradas. En esos casos, es mejor darte cuenta de tu error después de haber escrito algunos cientos de palabras, en lugar del último commit de un _pull request_ gigante. O peor aún, tras haber desplegado un cambio irreversible a producción.

Del mismo modo, los equipos crecen y cambian, y se nos olvidan las cosas. Si la empresa para la que trabajas es pequeña, no significa que lo seguirá siendo para siempre. Si empieza a crecer, seguramente empezarán a contratar nuevos empleados a un ritmo creciente y los RFCs les ayudarán a ponerse al día mucho más rápido. Adicionalmente, cuando uno de ellos te pregunte "¡¿Qué estabas pensando cuando agregaste esta funcionalidad?!", puedes enviarle un enlace al RFC sin tener que preocuparte por recordar cada detalle de cada decisión.

Sin embargo, algo que vale la pena recordar es que no todos los cambios necesitan un RFC. Algunos problemas son pequeños y auto-contenidos, y su solución puede ser obvia. En esos casos, un mensaje de _commit_ claro o una descripción completa de tu _pull request_ serían suficiente para documentar el cambio. Si alguien quiere aprender más sobre un archivo en particular, puede utilizar `git blame` para hacer un viaje en el tiempo y leerlos. 

No hay reglas matemáticas o completamente objetivas para decidir cuándo y cuándo no escribir un RFC. Cuando haya dudas, lo mejor que puedes hacer es preguntarle a tu equipo para entender sus expectativas.

## ¿Cómo?

Esta es una estrategia flexible que uso cuando es momento de empezar a trabajar:

1. Evalúa el espacio del problema y de la solución para decidir si se necesita escribir un RFC.
2. Identifica los coautores, colaboradores, interesados y tu audiencia.
3. ¡Empieza a escribir! Échale un vistazo a la [plantilla].
4. Enviálo y recibe comentarios y sugerencias.
5. Cuando hayas incorporado todas las sugerencias, asegúrate de que tu propuesta sea aceptada.
6. Impleméntala.

Tomemos un momento para hablar más de los puntos 4 y 5. Cuando envíes un borrador, es probable que recibas tantos comentarios sobre la _forma_ como sobre el _contenido_, como por ejemplo una nota que olvidaste borrar o una peticiones para que elabores una idea o para que incluyas más materiales visuales. Si en esa fase temprana se lo envías a muchas personas, recibirás muchos comentarios similares. Puede que una porción de tu audiencia no lean todo tu RFC si sienten que es un borrador inicial. Por eso, lo ideal es que refines tu RFC en estas fases iniciales con un grupo de personas que estén muy interesados en él, los cuales son normalmente tu equipo o tu _manager_. Una vez lo hayas hecho, puedes enviárselo a más personas. La nueva ola de comentarios debería consistir de preguntas y sugerencias sobre lo esencial del contenido. 

La aprobación de un RFC depende totalmente de las reglas o acuerdos que existan en tu organización. Algunos equipos necesitan firmas explícitas de las personas que lo revisen, mientras que otros están cómodos al interpretar la falta de sugerencias pendientes como aprobación implícita. De cualquier forma, asegúrate de todos estén en la misma página en este aspecto.

## En conclusión

Mantener una mente abierta es una parte esencial de todo el proceso. Al fin y al cabo, se trata de una **petición** de comentarios. Estás pidiéndole a otras personas que analicen tu propuesta. A algunos les va a gustar mucho, mientras que a otros no. Puede que esto último no se sienta muy bien, pero me he dado cuenta que en los casos donde se da esta división, las personas que no están convencidas desde el principio son las que más te pueden ayudar a elaborar una propuesta verdaderamente buena, asumiendo buena fe, claro está, y que nadie te está poniendo obstáculos para beneficio propio (si sospechas lo contrario, puede existir un gran problema de cultura organizacional).

Si alguien no está seguro de tu RFC, puede que haga falta considerar más alternativas. A veces, puede que alguien descubra un defecto importante en tu propuesta, causando que esta sea rechazada. Esto puede desalentarte, pero es en realidad un excelente desenlace. Esa persona te salvó de malgastar tu tiempo persiguiendo un objetivo que terminaría mal. Vuelve al punto de partida y analiza nuevas ideas teniendo en cuenta lo que aprendiste. O mejor aún, trata de que dicha persona se involucre como colaborador o coautor para que juntos trabajen en una propuesta más sólida.

[plantilla]: https://github.com/0x5d/0x5d.github.io/blob/main/content/posts/rfcs/files/plantilla-rfc.md
