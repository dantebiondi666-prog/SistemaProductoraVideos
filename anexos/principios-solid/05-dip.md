## Principio de Inversión de Dependencias (DIP)

## Propósito y Tipo del Principio SOLID:
El objetivo principal de esta técnica es lograr que los diseños de los softwares sean más comprensibles, flexibles y mantenibles. Su precursor Robert C. Martin la elaboró para aplicarse en el diseño orientado a objetos, aunque también puede ser implementado en metodologías como el desarrollo ágil o el desarrollo de software adaptativo.
En cuanto a uno de sus principios, "Dependency Inversion" (DIP), el autor refiere que "las entidades de software deben depender de abstracciones, no de implementaciones". Asimismo, los módulos de alto nivel no deberían depender de los de bajo nivel. Ambos deberían depender de abstracciones.

## Motivación:
Al depender de abstracciones y no de implementaciones concretas, el sistema gana: 
    
- Flexibilidad: sustituir componentes sin que los clientes que los consumen se vean afectados, ya que dependen de la abstracción y no de una implementación concreta
- Bajo acoplamiento: lo que facilita cambios en detalles sin romper el código
- Testeabilidad: facilita el testing
- Evolución: permite agregar nuevas funcionalidades
- Mantenimiento: el código termina siendo más limpio, lo que facilita su mantenimiento futuro
 
## Explicación de Clases Abstractas e Interfaces:
Una clase abstracta es una clase que no se puede instanciar directamente y sirve como plantilla para otras subclases. Características principales:
    
- No puedes crear objetos directamente de ella
- Puede tener métodos abstractos (sin implementación)  y métodos concretos (con implementación)
- Puede tener atributos y constructores
- Una clase solo puede heredar de una clase abstracta

    
En cuanto a las Interface podemos mencionar que es un contrato puro que define qué debe hacer una clase, pero no cómo lo hace. Es como un conjunto de promesas que una clase debe cumplir. Características principales:

- Solo define métodos (tradicionalmente sin implementación, aunque lenguajes modernos permiten métodos default)
- No puede tener atributos de instancia (solo constantes)
- Una clase puede implementar múltiples interfaces
- Define capacidades o comportamientos que una clase puede tener
    
La Inversión de Dependencias es un principio de diseño que establece que los módulos de alto nivel (la lógica de negocio importante) NO deben depender de los módulos de bajo nivel (los detalles de implementación). En cambio, ambos deben depender de abstracciones.

Invertir la dependencia significa cambiar la dirección de esa relación, insertar una abstracción (interfaz o clase abstracta) en el medio, de tal forma que:

- La clase de alto nivel depende de la abstracción
- La clase de bajo nivel también depende de (implementa) la abstracción

La "inversión" está en que ahora la clase concreta de bajo nivel debe adaptarse a lo que la abstracción define, en lugar de que la clase de alto nivel se adapte a los detalles concretos.

## Estructura de Clases:
 **Fuente PlantUML:** [01-solid-05-dip.puml](..\diagramas\01-diagrama-clases\01-solid-05-dip.puml)  
**Imagen PNG:** [01-solid-05-dip.png](..\diagramas\01-diagrama-clases\01-solid-05-dip.png)




## Justificación Técnica:
Sobre el diagrama solid-05-dip podemos observar:

Un módulo de alto nivel `ServicioNotificaciones` que tiene dependencias internas del tipo interfaz (`ICanalNotificacion` y `IRepoNotificacion`), el cual recibe abstracciones por inyección, no depende de estructuras de bajo nivel (implementaciones), las cuales pueden ser intercambiables.

Tambien se observa el bajo nivel (implementaciones) como ser `CanalEmail` y `CanalWhatsApp` que implementan a la abstracción `ICanalNotificacion`, `RepoSQLNotificacion` y `RepoMemoriaNotificacion` que implementan a la abstracción `IRepoNotificacion`.

→ Las abstracciones `ICanalNotificacion` y `IRepoNotificacion` son interfaces que expresan lo que el sistema necesita, sin atarlo a “cómo” se hace.
→ Las de bajo nivel (`CanalEmail`, `CanalWhatsApp`, `RepoSQLNotificacion` y `RepoMemoriaNotificacion`) son variantes intercambiables del “cómo” enviar/guardar.

En cuanto a las relaciones en UML, los módulos de alto nivel usan contrato (dependencia) hacia las abstracciones y estas ultimas, interfases, se cominican con las implementaciones. 


