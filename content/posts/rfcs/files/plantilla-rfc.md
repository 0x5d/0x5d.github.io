Esta plantilla es un complemento para mi artículo ["Sobre escribir RFCs"](https://0x5d.github.io/posts/es/rfcs/). Adáptala de la forma que consideres conveniente. ¡Si alguna sección no te parece útil, elimínala o cámbiala!

Asegúrate de revisar otras plantillas de RFC, tales como la de [Hashicorp](https://works.hashicorp.com/articles/rfc-template).

# RFC: (Título)

Autores: (Aquí va tu nombre)
Date:

## Resumen ejecutivo
Una descripción breve, en lo posible de un solo párrafo, donde describas el problema y la solución. Le ayudará a tus lectores a contextualizar el resto del documento.

## Contexto
¿Qué antecedentes o trasfondo sería util que los lectores conozcan? Regístralos aquí. Puede ser de caracter histórico (por ejemplo, "nuestra base de usuarios se ha incrementado") o técnico.

### Propósito/ Justificación
¿Por qué solucionar este problema? ¿Cuál es el principal beneficio?

## Requisitos
Una lista de requisitos ayuda a delimitar el espacio de la solución, facilitando la justificación de tu propuesta. Pueden ser de tipo funcional y no funcional.

## Propuesta

### Descripción
Aquí va una descripción de tu propuesta a grandes rasgos. Al terminar de leerla, tu audiencia debería quedar con una imagen clara de su dirección, alcance, y resultados esperados.

Como material de apoyo puedes incluir diagramas de flujo, bocetos, o prototipos de interfaces, ya que éstos ayudan a visualizar la solución desde puntos de vista diferentes.

### Diseño detallado
Esta es la sección donde vas al detalle y describes todos cambios necesarios para llevar a cabo tu solución. No temas ser preciso y específico; las secciones anteriores daban una imagen general del rompecabezas, pero aquí es donde debes mostrar cuáles son las piezas y cómo encajan todas juntas.

### Operaciones
El propósito de esta sección es responder preguntas tales como "¿Cómo desplegaremos la solución?", ¿Cómo va a ser la migración?", etc. Trata de anticipar escenarios que sucederán durante el ciclo de vida de tu solución: actualizaciones, estrategias de recuperación, o situaciones de emergencia donde sea necesaria la intervención manual.

### Métricas
Describe todas las métricas que deberán ser monitoreadas para verificar el buen funcionamiento de la nueva funcionalidad o el nuevo comoponente.

### Costo
¡Nada es gratis! ¿Cuál será el costo de implementar tu propuesta? Piensa en cuántas personas deberán trabajar en ella y por cuánto tiempo. Calcula un estimado de los costos ocasionados por el uso de infraestructura adicional, el pago de servicios de terceros, o incluso el costo de oportunidad si, por ejemplo, al priorizar esta implementación hay un riesgo de perder clientes.

### Riesgo
Describe cualquier factor que pueda poner en peligro tu plan, junto con una estrategia de mitigación.

## Alternativas

En esta seccion debes listar las opciones que evaluaste. La primera debe ser siempre "No hacer nada", y es el complemento de la sección de "Propósito": mientras que ésta última trata sobre explorar el resultado ideal, en "No hacer nada" debes pensar en las consecuencias de mantener todo igual.

Normalmente hay una o dos alternativas además de "No hacer nada". Si no las has identificado, puede ser una señal de que debes explorar más el espacio de la solución.

## Referencias

Lista el material externo que consultaste para que quien revise tu RFC pueda verificarlo.

## Apéndices

Aquí va cualquier información o explicación suplementaria, la cual puede que sea útil solo para un porcentaje menor de tu audiencia; material que quieras incluir "por si acaso".
