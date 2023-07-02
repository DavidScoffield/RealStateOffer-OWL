# Ontología RealStateOffer OWL

## Objetivo

- Desarrollo de una ontología para la descripción de ofertas inmobiliarias.

- Objetivo didáctico

- Diseñar y desarrollar una ontología básica sobre oferta inmobiliaria.

### Requerimientos

Se desea modelar la base de conocimiento para un observatorio inmobiliario. En este observatorio es importante modelar diferentes características que describen a los inmuebles de un sector geográfico específico.

Existen diferentes tipos de inmuebles como pueden ser lotes, casas, departamentos, galpones, etc. Estos inmuebles se encuentran ubicados en una dirección postal con una altura determinada. Además, se emplazan sobre un lote. Los lotes pueden ser regulares o irregulares. Los lotes se encuentran emplazados con un frente y un contrafrente. El frente da a una calle, y el resto de los lados del lote pueden lindar con otros lotes o con otra calle. Tambien poseen una latitud y longitud que remite al frente del mismo.

Por su parte, las casas, aunque están construidas sobre un lote, se consideran un tipo de inmueble específico, al igual que los departamentos y los edificios. Un edificio de departamentos es un tipo de inmueble que posee varios departamentos. Lo interesante de los departamentos es que pueden estar ubicados en la misma latitud y longitud pero difieren unos de otros por ejemplo por la altura. (Piense en todos lo que se puede describir de una casa y de un departamento). De los inmuebles se sabe tambien que servicios tienen, por ejemplo agua corriente, cloacas, gas natural, tendido eléctrico, etc.

Además de las características “morfológicas” de los inmuebles, existen características comerciales. Para determinar el precio de un inmueble se acude al mercado inmobiliario de ofertas del tipo avisos. Es por eso, que los inmuebles poseen publicaciones en avisos en los que se indican diferentes características de los mismos. Entre ellas, el precio y la moneda, la fecha en que se realiza la publicación, la inmobiliaria que gestiona la venta, si es un inmueble apto para solicitar un crédito hipotecario, etc.

### Tarea

Desarrollar una ontología de conocimiento que permita poblar una base de conocimiento que incluya diferentes elementos de la oferta del sector inmobiliario. La misma debe incluir, al menos, los tipos de inmuebles lotes, departamentos y casas. De estos deben indicarse las características de los mismos, los servicios que se encuentran en la ubicación, los valores de venta, monedas y posibles variaciones y las inmobiliarias que los gestionan.

La ontología, una vez poblada, tendrá capacidad de resolver a través de sus entidades, propiedades de objeto y de datos un mínimo de preguntas según se señalan en adelante:

    ¿Cuáles son los inmuebles con superficie de lote mayor a una cantidad de metros cuadrados, que se encuentre en un barrio con servicios de luz, gas y cloacas?

    ¿Qué inmobiliaria posee inmuebles para un perfil de vivienda dado?

Escriba las consultas SPARQL que permita responder a las preguntas anteriores.

Asegure que su diseño posee al menos 3 ejemplos de las siguientes características de propiedades y justifique.

- Transitiva
- Simétrica
- Asimétrica
- Reflexiva
- Irreflexiva
- Funcional
- Inversa funcional

Defina dos clases utilizando axiomas de clases. Si de los requerimientos no se le ocurren los casos, puede crear nuevos requerimientos para ello.

Utilizar la herramienta **Protege** en la definición y asistencia en el diseño de una ontología.

Escribir la ontologia en un **archivo tourtle**.

> Agregar las consultas en la descripción de la ontología.
