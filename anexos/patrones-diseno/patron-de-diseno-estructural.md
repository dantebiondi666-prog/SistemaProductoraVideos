# Anexo - Aplicación de Patrón de Diseño Estructural - Nombrepatronelegido

## Patrones de Diseño Estructural y su relación con SOLID

*Los patrones estructurales permiten organizar la arquitectura interna de un sistema, reduciendo el acoplamiento entre clases, promoviendo la composición por sobre la herencia y permitiendo extender funcionalidades sin modificar código existente, alineándose fuertemente con OCP y DIP.

El patrón Facade proporciona una interfaz simplificada para un conjunto de subsistemas complejos.
Permite ocultar la complejidad interna y exponer un único punto de acceso para las operaciones principales del sistema.*

## Propósito y tipo del Patrón

### Propósito:

En el sistema de la Productora de Videos, la creación y gestión de un proyecto de video requiere interactuar con múltiples clases:

. GestorVideos

- GestorUsuarios

- GestorProyectos

- ServicioRender

- ServicioPublicacion

Esto generaba:

- Fuertes dependencias entre capas.

- Dificultad para mantener la UI y los casos de uso.

- Duplicación de llamadas a servicios.

El patrón Facade unifica estas operaciones en una única clase coordinadora:
SistemaProductoraFacade.

### Tipo:

...

---

## Motivación

*Aquí se detalla el problema en profundidad, explicando: ● Cómo funcionaba originalmente el sistema y las limitaciones detectadas. ● Qué clases estaban involucradas y cómo interactuaban. ● Por qué este diseño generaba problemas de mantenibilidad, escalabilidad o rigidez. ● Qué nuevas clases se incorporan con el uso del patrón seleccionado y cuál es su función. ● Cómo el patrón de diseño reorganiza la arquitectura para resolver el problema. (Agregar párrafos explicativos aquí.)*

## Estructura de Clases

*No es necesario incluir todas las clases del proyecto en el diagrama, sino únicamente aquellas que participan directamente en la implementación del patrón. Esto permite mantener un diagrama claro, conciso y centrado en la arquitectura relevante para la aplicación del patrón. A continuación se presenta el diagrama UML del diseño aplicado:*

*IMG-DIAGRAMA*

*Ver diagrama en tamaño completo.*

## Justificación Técnica de la Estructura de Clases

*En esta sección se detalla la explicación técnica del diagrama UML presentado anteriormente. El objetivo es justificar las clases incluidas y su rol dentro de la solución implementada mediante el patrón creacional. (Completar con los siguientes puntos:) ● Descripción detallada de cada clase incluida en el diagrama , indicando: ○ Su responsabilidad dentro del patrón. ○ Su relación con otras clases. ○ Por qué es necesaria para aplicar correctamente el patrón. ● Explicación del flujo de creación de objetos: Describir cómo las clases colaboran entre sí para resolver el problema de creación. Mencionar qué clase inicia el flujo, cuál delega la responsabilidad y cuál instancia los objetos finales. (Agregar la explicación técnica correspondiente aquí.)*