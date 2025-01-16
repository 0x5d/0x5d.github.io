---
title: "Pantanos"
date: 2025-01-12T15:11:47-05:00
lastmod: 2025-01-12T15:11:47-05:00
publishDate: 2025-01-12T15:11:47-05:00
etiquetas: ["carrera", "management", "liderazgo"]

ShowReadingTime: true
ShowToc: true
---

Desde hace un par de años, a medida que he tenido la oportunidad de liderar equipos en proyectos y situaciones más complejas e inciertas, me ha picado la curiosidad por entender más sobre management de ingeniería y liderazgo técnico. Por eso he leído 3 libros hasta el momento: [The Manager's Path](https://www.oreilly.com/library/view/the-managers-path/9781491973882/), de Camille Fournier, y [An Elegant Puzzle](https://press.stripe.com/an-elegant-puzzle) y [Staff Engineer](https://staffeng.com/book) de Will Larson.

Recomiendo muchísimo leerlos, aún si no estás interesado en convertirte en manager (por mi parte también siento que me falta mucho por aprender en el lado técnico): aprender sobre management me ha ayudado a entender mejor qué debo esperar de mis managers y qué esperan ellos de mí, y también he aprendido muchos tips de liderazgo que se pueden aplicar en general.

Sin embargo, me parece que estos libros hablan sobre el progreso en la carrera en situaciones más o menos ideales. En mi experiencia, estos escenarios perfectos que asumen implícitamente (organizaciones racionales y 100% transparentes, personas razonables, no egoístas y totalmente alineadas con la visión de la empresa, recompensas seguras por hacer bien el trabajo, etc.) no son tan comunes. Ciertamente, navegar una organización ejemplar sería muy fácil, pero la realidad es que las empresas tienen falencias que pueden estancar nuestro progreso seriamente.

Las únicas fuentes que he visto que se enfocan en los intereses individuales de una ingeniera, admitiendo que el contexto organizacional puede estar lejos de ser perfecto son
- [Know How Your Org Works](https://copyconstruct.medium.com/know-how-your-org-works-or-how-to-become-a-more-effective-engineer-1a3287d1f58d), de Cindy Sridharan,
- [Being Glue](https://www.youtube.com/watch?v=0NUhO9r8UUc) de Tanya Reilly.

Son también las únicas fuentes entre las que he visto cuyas autoras no eran managers al momento de escribirlas.[^1]

No hablo de situaciones "sencillas" de manejar, tales como malentendidos particulares, coyunturas temporales (como la renuncia de tu manager), o problemas obvios (como que un colega te haga bullying) sino de patrones sistemáticos que, a menos que se identifiquen rápido, te pueden poner en una posición desventajosa de la cual es muy difícil salir. Es esta dificultad para navegarlos y encontrar una salida la razón por la que me refiero a estos estados como "pantanos".

Estos son patrones que he identificado a lo largo de mi carrera que han hecho que yo, u otras personas de otros equipos, entren en un pantano. Algunos los he vivido, y otros los he observado desde una distancia segura.

Antes de pasar a hablar de ellos, quiero aclarar que la mayoría de las soluciones que propongo son de caracter individualista. Es decir, son algunas cosas que puedes hacer **tú** si te encuentras en una situación parecida. Claramente, una solución a nivel organizacional sería ideal, pero una organización puede tomar años en cambiar, por lo que hay que pensar bien si ese es el camino que se quiere tomar.

También quiero aclarar que en general, estas situaciones no suceden como parte de una estratagema compleja planeada por años para dañar tu carrera. A veces solo se trata de una cuestión de incentivos y cómo una organización o un equipo responden a ellos. A veces son causadas por la inexperiencia de un líder, y tú solo eres daño colateral. La razón por la que quiero hablar de estos patrones es para ayudarte a identificarlos antes de que tu carrera se vea afectada por ellos.

![Pantano de Head Lopper](https://static1.cbrimages.com/wordpress/wp-content/uploads/robot6/2013/10/HL1-preview-4.png?q=50&fit=crop&w=750&dpr=1.5)
> Andrew MacLean, Head Lopper #1

## Distribución desequilibrada de equipos

En este patrón hay al menos 2 grupos de personas en un mismo equipo que comparten responsabilidades operativas con una diferencia horaria importante (al menos ~6 hrs). Si uno de los grupos tiene menos miembros, la carga operativa en ese grupo va a ser mayor por persona, afectando negativamente su productividad y sus oportunidades de progresar en la empresa.

Digamos que hay un equipo con 8 personas en América, y 4 personas en Europa, encargados de desarrollar y soportar una aplicación web. A cada persona le corresponde una semana de on-call (_guardia_) primario, y una de secundario inmediatamente luego. Los turnos de on-call duran 12 horas, y el turno de on-call de las personas en Europa inicia cuando termina el de las de América. A simple vista, éste parece ser un acuerdo justo, no?

Con estas condiciones, las personas en América tendrían una semana de on-call primario, una de on-call secundario, y luego 6 semanas para trabajar en funcionalidades y mejoras (ojalá) sin distracciones.

Por otro lado, el equipo en Europa pasaría 2 semanas on-call y 2 semanas trabajando en mejoras. Esto significa que el 50% de su tiempo lo pasan dando soporte, comparado con 25% para las personas en América!

Para un manager, esto se ve como una mejora del 25% en productividad en el equipo de América. Por lo tanto, es probable que las personas en ese equipo avancen hacia ascensos y puestos de liderazgo mucho más rápido. A su vez, dado que son más productivos, los managers empezarán a entregarle más responsabilidades a estos líderes, quienes seguro querrán que sus equipos crezcan, posiblemente exacerbando el problema si contratan más personas en su zona horaria.

Por su lado, las personas en Europa entrarán en un pantano. Dada su menor productividad, los managers le asignarán a sus miembros proyectos de menor impacto y alcance, lo cuál hará más difícil justificar un ascenso o aumento. La moral del equipo tenderá a disminuir también, afectando aún más la productividad.

La situación puede empeorar con otros factores, tales como
- Concentración de trabajo reactivo en una zona horaria: En el ejemplo de arriba, se asume que ambos equipos tienen la misma carga mientras están on-call. Sin embargo, podría pasar que la mayoría de usuarios estén en Europa, de modo que que los turnos de soporte de América (que suceden durante tarde/ noche en Europa) sean tranquilos y permitan que aún durante sus turnos de on-call, el equipo pueda seguir trabajando en funcionalidades e iniciativas nuevas.
- Burnout: Dependiendo de la frecuencia de la rotación, en casos extremos como el del equipo de Europa, puede que el tiempo transcurrido entre semanas de on-call no sea suficiente para que el equipo se recupere del cansancio físico y mental que puede suponer estar en guardia por 12 horas o más durante 2 semanas, haciendo que la moral y la productividad bajen cada vez más rápido.
- Ubicación: Si la empresa tiene oficinas, y el equipo más pequeño es remoto, sus oportunidades de alcanzar y seguir el ritmo del otro equipo serían aún menores, puesto que la comunicación asíncrona tiende a tener un ancho de banda mucho menor.

### Señales de alerta
- Crecimiento desproporcionado en un continente diferente al tuyo; o, si vas a entrar a la empresa, presencia desproporcionada (en términos de número de empleados), en otro continente.

### Solución
Primero hay que entender la fuente del desequilibrio. Puede que el equipo de liderazgo no tenga mucha experiencia, pero también puede que se trate de una situación transitoria; si por ejemplo la empresa se está expandiendo a otros continentes, se esperaría que el equipo más pequeño siga creciendo, eventualmente zanjando la brecha.

Resolver una distribución desequilibrada de equipos requiere del apoyo de managers y directores, y probablemente de VPs (dependiendo del tamaño de la empresa). En caso de que ya haya un desequilibrio, se debe limitar la contratación al continente con el equipo más pequeño. Si no, se debe tratar de mantener un factor de miembros en cada continente de 1:1 a medida que se contratan más personas. Conseguir el apoyo de líderes puede ser difícil si éstos se encuentran en el mismo continente del grupo con más integrantes, pues es probable que allí esté su red profesional. Para ganar ese apoyo, es necesario mostrarle a tu manager datos claros sobre cómo la distribución del equipo está afectando tu productividad, pero también las iniciativas que vas a trabajar cuando se solucione - es de bastante ayuda mostrar que ya tienes planeado invertir bien el tiempo que recuperarás.

Si encuentras demasiada fricción, puede que simplemente tu equipo no sea una prioridad para la estrategia de la empresa, o que se vea como un equipo 100% de soporte. Si crees que ese puede ser el caso, corrobóralo y considera la opción de buscar trabajo en otro lugar, o pedir una reubicación.

## El equipo de limpieza

El equipo de limpieza surge cuando la organización recompensa mucho más el lanzamiento de nuevos proyectos o funcionalidades que su estabilidad. Esto lleva a que las personas que hayan entregado _algo_ rápido sin terminarlo del todo se asignen a nuevas iniciativas, las cuales entregan rápido, repitiendo el ciclo, mientras que otro grupo de personas - el equipo de limpieza - es asignado a mantener o terminar el trabajo que estos primeros dejan. Dados los incentivos de la organización (más cosas nuevas es mejor), el primer grupo será recompensado mientras que el segundo tendrá peores evaluaciones y cada vez menos acceso a nuevas oportunidades. 

En mi experiencia, en una organización saludable cada líder se enfoca en un proyecto o área de los cuales son responsables, y siguen trabajando por meses o incluso años en mejorarlos. Este enfoque prolongado en un área hace posible que nos hagamos cargo personalmente de las consecuencias de nuestras decisiones. Además, dado que un líder está ocupado con un área específica, se crean oportunidades para que otras personas desarrollen nuevas iniciativas por fuera de ella y crezcan también.

{{< twitter user="charliermarsh" id="1878081371907694967" >}}
> _Este tweet de Charlie Marsh encapsula bien lo que quiero decir, cambiando "company" por "area"._

Sin embargo, he visto como algunos managers desarrollan una predilección por ciertas personas talentosas, a quienes mueven impacientemente de un proyecto a otro, a veces sin que éstos estén completos. Las consecuencias de ésto son en general positivas para dichas personas, dado que en cierto modo están acumulando logros que luego pueden llevar a aumentos o ascensos. Sin embargo, a su paso van dejando sistemas incompletos de los que otras personas deben hacerse cargo. Este último grupo de personas se convierte en un "equipo de limpieza",encargado de estabilizar y mantener los proyectos que quedan empezados, a tal punto que esta se vuelve su responsabilidad. En este caso, el pantano se crea por la cantidad de trabajo que supone hacerse cargo de dichos proyectos, al punto que el equipo de limpieza se queda sin capacidad para trabajar en iniciativas propias.

El equipo de limpieza enfrenta entonces un dilema incómodo: si eligen no mantener o terminar estos proyectos, pueden ir dañando la relación con su manager poco a poco; pero si aceptan, sacrifican sus propias iniciativas por trabajar en las de otros, y puede que les toque responder por las malas decisiones pasadas de otra persona. Aceptar puede también llevar a otros problemas, como convertirse en **el camino de menor resistencia** (descrito más adelante).

Este patrón se perpetúa si la empresa recompensa más el comienzo de nuevos proyectos que los esfuerzos de estabilización. Si en algún momento hay algún problema con los proyectos que van dejando a su paso, es probable que por el tiempo transcurrido nadie recuerde las circunstancias iniciales, y se le achaquen las fallas a las personas que estén a cargo del sistema o componente en ese momento. Desde el punto de vista de un director o un VP, parecerá que el manager y su ingeniero líder llevan una racha increíble, y que el resto de personas tiene un rendimiento menor.

{{< youtube -c4CNB80SRc >}}
> _Steve Jobs on living with the consequences of your choices._

### Señales de alerta
- Los proyectos nuevos siempre son asignados o tomados por un par de personas.
- Experimentas mucha resistencia por parte de tu manager cuando propones iniciativas en las que quieres trabajar.
- Tú u otras personas reciben muchas solicitudes para mantener sistemas en cuya construcción no estuvieron involucrados.
- Las evaluaciones de rendimiento revelan un SOS ([_Shiny Object Syndrome_](https://en.wikipedia.org/wiki/Shiny_object_syndrome), por sus siglas en inglés), recompensando mucho más features y proyectos nuevos sobre esfuerzos de estabilización de sistemas existentes.

### Solución
Como en todos los casos, el primer paso debe ser hablarlo con tu manager. Muéstrale las ocasiones en las cuales tu rango de oportunidades se vio limitado por tener que trabajar completando trabajo ajeno. Puede que tu manager no tenga mucha experiencia y haya generado un "equipo de limpieza" no por mala fé, sino por no haber identificado las consecuencias de segundo orden que se desprenden de sus decisiones. Si es una persona razonable y tu evidencia es concreta, seguro reaccionará.

Si no lo hace, la desobediencia puede funcionar. Por ejemplo, si hay un proyecto en el que quieres trabajar, pero no puedes porque tu _backlog_ está lleno, o tu manager se rehúsa a que cambies tus prioridades, puedes elegir trabajar en él independientemente. Es una estrategia arriesgada, ya que igual debes entregar el resto del trabajo, por lo que probablemente tendrás que trabajar en el proyecto en tu tiempo libre. Además, no tienes ningún tipo de garantía de que recibirás algún tipo de recompensa a su término. Habiendo dicho eso, he visto que esta estrategia ha funcionado bastante bien. Si no descuidas tu trabajo actual y tu proyecto es de alto impacto, tu empresa y tu manager no podrán simplemente ignorarlo.

Por último, la mejor solución es la prevención. Establecer límites claros para tu trabajo puede ser difícil (sobre todo si eres nuevo en un equipo y quieres agradarle a la mayoría de personas), pero te puede ayudar mucho en el largo plazo.

## El camino de menor resistencia

El sentido del deber de algunos ingenieros, o la falta de asertividad en su comunicación, los puede llevar a siempre aceptar peticiones para trabajar en tareas que el resto del equipo desdeña, pero que son de vital importancia, como entrevistas, actualizaciones de componentes, la solución de problemas en sistemas críticos, entre otras. Si la organización no recompensa este trabajo o no sabe evaluarlo, esto puede crear un ciclo donde la mayoría del tiempo de un ingeniero sea ocupado por tareas que no le ayuden a mejorar su posición, a pesar de que sus esfuerzos multiplican la productividad del resto del equipo.

En mi opinión, los mejores ingenieros son quienes no se preocupan por qué tan glamuroso es el trabajo. Si hay algo que debe hacerse, se debe hacer y punto. No importa si se trata de arreglar el sistema de entrega continua para desbloquear un despliegue, o de implementar una nueva arquitectura. Son personas en las que el equipo y la organización pueden confiar.

Por esto mismo, se pueden convertir en el camino de menor resistencia para su manager o su equipo. Por su altruismo son propensos a aceptar realizar el trabajo que nadie quiere hacer, solo por su fuerte sentido de deber. En una organización saludable, este tipo de trabajo es recompensado (siempre y cuando su impacto sea real), y en muchas organizaciones incluso se espera que sean los ingenieros en un nivel senior o superior quienes se encarguen de él directamente, o al menos de coordinarlo. Will Larson describe esto en más detalle en _Staff Engineer_.

El problema aparece cuando el sistema de evaluación del rendimiento no está diseñado para tomar en cuenta este trabajo "de soldadura" (_glue work_[^2]) por Tanya Reilly. La metáfora viene de que - tal como la soldadura o el pegamento - este trabajo no es inmediatamente visible a pesar de que juega un papel crítico en mantener la estructura y los procesos de la empresa o equipo. Cualquier organización siempre tendrá montones de "piezas por soldar": migraciones, actualizaciones, entrevistas, revisiones de código, documentos de diseño, reuniones de planeación, etc. Si una organización valora un conjunto de actividades totalmente diferentes, y tú estás haciendo el trabajo de soldadura que todos evitan, puedes arriesgarte a terminar en un pantano.

Poco a poco las personas asociarán cierto tipo de tareas contigo, y te las delegarán directa o indirectamente. Esto no lo harán necesariamente con malas intenciones - simplemente tú eres el camino de menor resistencia, lo cual ha creado sesgos en su forma de pensar.

### Señales de alerta
- Muchas personas - incluyendo tu manager - constantemente redirigen trabajo o preguntas hacia ti sobre áreas o temas que son "de bien común", de las cuales más de una persona debería estar a cargo.
- Te asignan tareas de ciertas categorías solo a ti, a pesar de que hay otras personas en total capacidad de hacerlas. Por ejemplo, entrevistas, trabajos operativos como actualizaciones de componentes, arreglos en el sistema de CI/CD.
- El trabajo de soldadura que hiciste en el último año o semestre no tuvo un impacto positivo en tu evaluación de rendimiento pasada.

### Solución
Lo primero que debes probar es decir "no". Puede que te hayas vuelto un experto en soldar, y que una tarea que a ti te llevaría 5 minutos a otro le tome 1 hora, pero es necesario que empieces a rechazar peticiones para hacer más.

Dependiendo de qué tan "espeso" sea el pantano en el que te encuentres, puede ser más difícil, pues entre más grande sea el sesgo en las personas que te rodean, tendrás que responder "no" más veces. Esto puede ser desgastante, pero hay que restablecer las expectativas que tiene tu equipo de ti. Además, negarte a trabajar en tareas en las cuales te han encasillado crea oportunidades para redistribuir ese conocimiento y experiencia. A tu manager seguro le gustará reducir el [bus factor](https://es.wikipedia.org/wiki/Bus_factor) del equipo.

Documenta todo lo que sepas sobre las tareas que quieres redistribuir en tu equipo, y anúncialo en un canal público (email o Slack). Ten en cuenta que el cambio no será inmediato; si te hacen preguntas sobre temas ya documentados, recuérdales dónde pueden encontrar la respuesta. Si lo que quieren saber no está todavía en la documentación, ¡pídeles que contribuyan! Por otro lado, dale visibilidad a un proyecto de largo alcance en el que estés trabajando, y utilízalo como razón para rechazar el trabajo adicional. Un "no" a veces es difícil de procesar para otras personas cuando estas tienen una concepción afianzada sobre ti, mientras que un "no puedo porque estoy ocupado" es más fácil de digerir.

Contar con el apoyo de tu manager, de modo que él o ella ayude a interceptar peticiones dirigidas hacia ti y redistribuirlas de manera más equilibrada entre otros miembros del equipo, es un impulso significativo. Por último, asegúrate de que el trabajo nuevo que estarás haciendo en lugar de "soldar" sea considerado valioso y de alto impacto, para que te acerque a tu meta, ya sea un ascenso o un aumento.

Vale la pena decir que debes tener cuidado de no sobrecompensar, pasando a rechazar cualquier petición de ayuda. Sigue participando en trabajo de soldadura, pero anota todo ese esfuerzo (con un impacto correspondiente cuantificable), de modo que no se te olvide en tu siguiente evaluación de rendimiento.

## Evaluaciones asintóticas

Las evaluaciones asintóticas son aquellas en las que los criterios de aprobación son inalcanzables, creando una dinámica de ["Aquiles y la tortuga"](https://es.wikipedia.org/wiki/Paradojas_de_Zen%C3%B3n) o de "[la zanahoria y el bastón](https://en.wikipedia.org/wiki/Carrot_and_stick)".

Aplicado a tu carrera, esto significa que tus esfuerzos difícilmente serán recompensados de forma proporcional, puesto que la meta es inalcanzable. De esta lista, es el único patrón que creo que se usa conscientemente de manera rutinaria. En mi carrera he visto diferentes encarnaciones de este patrón:

#### Evaluaciones de rendimiento relativas
Consisten en identificar los mejores empleados (según algún criterio) para luego comparar al resto contra ellos.

Por ejemplo, se escogen las mejores personas en cada nivel de una organización, para luego compararlas con quienes que se encuentren en el nivel inferior a ellas y decidir si éstas últimas merecen un ascenso.

Hay una versión más extrema, llamada _stack ranking_, donde después de cada evaluación, se determinan también los empleados con más bajo desempeño para despedirlos.

En entornos altamente competitivos, donde todos están tratando de hacer _mejor_ la misma actividad estas prácticas _podrían_ tener sentido (por ejemplo, equipos de ventas o de deportes individuales). A mi parecer, sin embargo, es un método nefasto para evaluar el trabajo de ingenieros de software, que a menudo deben trabajar en equipo, coordinar y colaborar entre ellos para lograr un bien común, realizando tareas completamente diferentes (si dos ingenieros en tu equipo están haciendo lo mismo, más vale que haya una buena razón). Utilizar evaluaciones relativas puede crear ambientes hípercompetitivos donde la colaboración se ve como un riesgo, y puede acabar con el tiempo libre de los empleados que quieran lograr un ascenso o un aumento.

El artículo de Doug Meil, ["Stack Ranking: Organizational Cancer"](https://cacm.acm.org/blogcacm/stack-ranking-organizational-cancer/) explica los antecedentes y las consecuencias del stack ranking. 

Una vez le pregunté a un manager su opinión sobre qué me faltaba para un ascenso, durante un 1:1 luego de una ronda de evaluaciones de rendimiento. Su respuesta fue "Pues A y B (otros ingenieros) son las únicas personas en ese nivel... La barra está puesta muy alta". Las 2 personas que mencionó eran personas muy talentosas, pero se sabía que trabajaban 12 horas diarias o más constantemente. El mensaje subyacente parecía ser: "si quieres ascender, sigue su ejemplo". Afortunadamente puse mi bienestar por encima de mi ambición - mi experiencia hasta ese momento decía que el incremento en mi salario no sería suficiente para compensar la fatiga mental y física acumulada por trabajar a ese ritmo durante un año entero.

En retrospectiva, esta debió haber sido mi señal de que ese no era ya un equipo donde podría seguir creciendo de una manera sostenible, y que debí haber buscado irme a otro equipo o empezar a buscar otro trabajo.

Lo peor de todo es que la empresa en la que estaba tenía criterios claros para cada nivel en la carrera de ingeniería, pero mi manager por alguna razón no eligió seguirlos. Tu manager debería ser tu mayor entusiasta. Si no tienes su apoyo para seguir avanzando en tu carrera, será muy difícil hacerlo a menos que cambies de equipo o de empresa.

#### Criterios vagos o inexistentes

Los criterios de evaluación abstractos hacen que sea más fácil crear razones para no dar un aumento. Por ejemplo, si un requisito para que avances al siguiente nivel es "Liderar proyectos de alto impacto", una posible razón puede ser que el impacto de tus iniciativas no haya sido suficiente, o que no lideraste suficientes proyectos.

### Señales de alerta
- Tus líderes se basan exclusivamente en las comparaciones con otras personas para evaluar tu rendimiento.
- El equilibrio de trabajo/ vida en la organización está patas arriba.
- Tu organización no tiene un plan de carrera con criterios claros para pasar de un nivel al siguiente.

### Solución

Si solo tu manager está aplicando evaluaciones relativas o _stack ranking_, trata de hacerle ver que no es una estrategia sostenible si quiere mantener un equipo de ingeniería estable. 

_A veces_ las evaluaciones relativas en equipos de software surgen como resultado de sesgos inconscientes, no necesariamente como parte de un plan premeditado para aumentar la competencia entre ingenieros. Por ejemplo, un manager puede tener personas predilectas en su equipo, y al evaluar al resto de personas, en lugar de responder "¿Qué tan bueno es el trabajo de esta persona?" (una pregunta que implica cálculos difíciles), responde "¿Qué tan bien me cae esta persona comparada con mi empleado favorito?".

Es por esto que es importantísimo llevar un registro de tus logros, correlacionados con un impacto medible y verificable. Por ejemplo, "Investigué e implementé técnicas para mejorar los tiempos de carga de la consola de métricas en un 30%" (añade un link a un reporte o [documento de diseño](https://blog.0x5d.io/es/posts/rfcs/)). En este caso, un líder responsable y con la suficiente autoridad se dará cuenta de su error y tratará de corregirlo.

Dicho eso, hay que tener en cuenta que a veces las manos de tu manager estarán atadas, cuando se trate de políticas que vienen de arriba. En ese caso, tu mejor apuesta es trabajar con él para identificar proyectos cuyo impacto sea incontrovertible.

Las evaluaciones 360 (las cuales incorporan la retroalimentación de tus pares, no solo la de tu jefe), pueden sonar como una posible solución, pero normalmente:
- Tienen un efecto nulo sobre tus oportunidades, ya que en general tanto tú como tus colegas no tienen ningún incentivo para hablar mal el uno del otro, por lo que los feedbacks terminan siendo neutros o vagos.
- Tienen un efecto negativo sobre la organización, ya que demandan de demasiado tiempo para agregar todo el feedback, calibrarlo, analizarlo, etc., y distraen de otras acciones que podrían tener resultados más tangibles.

["Hell is Other People"](https://open.substack.com/pub/lcamtuf/p/hell-is-other-people-performance) de Michał Zalewski profundiza más sobre las promesas fallidas de las evaluaciones 360.

Sin embargo, hay personas que están convencidas de que este tipo de prácticas son efectivas. En ese caso debes preguntarte si estás dispuesto a trabajar en constante competencia con tus pares para avanzar en tu carrera en tu empresa actual.

# Observaciones finales

Si todavía no has entrado a un pantano, o no te has encontrado con ninguno de estos patrones, espero que este artículo te ayude a evitarlos en el futuro.

Si en este momento te encuentras en una situación compleja, recuerda que siempre hay una salida. Hay circunstancias en las cuales nos sentimos acorralados, pero es en esos momentos en los que hay que hacer un alto, respirar, y analizar la situación estratégicamente. Las soluciones que propongo aquí no son exhaustivas ni infalibles, pero espero que te den un punto de partida para comenzar a trazar tu propio camino.

Lo más importante es recordar que tu carrera es tuya. Si bien es valioso ser un buen miembro del equipo y contribuir al éxito de tu organización, esto no debe venir a costa de tu propio desarrollo profesional o bienestar personal. A veces la solución más saludable es reconocer que el ambiente actual no es propicio para tu crecimiento y buscar nuevas oportunidades.

Estas son algunas de las situaciones que he vivido o que he visto en personas cercanas.  Si has encontrado otros patrones de management u organizacionales que resulten en pantanos, me gustaría conocerlos también. Puedes encontrarme en [Twitter](https://x.com/_0x5d), [Bluesky](https://bsky.app/profile/0x5d.bsky.social) o por [email](mailto:blog@0x5d.io)!


[^1]: Una posible razón por la que escaseen más perspectivas realistas como la de Cindy Sridharan es que, en mi opinión, la mayoría de ICs o líderes no tienen ningún incentivo para admitir errores en público. Desde el punto de vista de un IC, hablar sobre su empleador actual, así sea omitiendo información específica, puede traerle consecuencias negativas. Para un manager, por otro lado, admitir que actuó de manera egoísta o propició ambientes que no se alineaban con los intereses de su empresa o de su(s) equipo(s) puede costarle su carrera.

[^2]: Sé que esta traducción no es literal, pero "trabajo de soldadura" es mejor que "trabajo de pegamento", o "trabajo de adhesión" (muy abstracto para mi gusto).
