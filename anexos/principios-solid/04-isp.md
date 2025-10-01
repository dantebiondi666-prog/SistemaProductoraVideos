# Principio de Segregación de Interfaces (ISP)

## Propósito
El Principio de Segregación de Interfaces (ISP) establece que **ninguna clase debe estar forzada a implementar métodos que no utiliza**.  
En otras palabras, es preferible **tener varias interfaces pequeñas y específicas** en lugar de una única interfaz "gorda" con demasiadas responsabilidades.

## Motivación
En el sistema *SistemaProductoraVideos*, inicialmente podríamos tener una interfaz general `IGestionMultimedia` que obligue a todas las clases a implementar métodos de subida, edición, comentarios y monetización de videos.  
El problema es que **no todas las clases necesitan todos esos métodos** (por ejemplo, un *UsuarioEspectador* solo reproduce y comenta, pero nunca sube videos).

Esto genera **acoplamiento innecesario** y clases con métodos vacíos o mal implementados.

Aplicando ISP, dividimos esa interfaz en varias más pequeñas y cohesivas:

- `ISubirContenido` → para quienes suben videos.  
- `IReproducirContenido` → para quienes consumen videos.  
- `IComentarContenido` → para quienes dejan comentarios.  
- `IMonetizarContenido` → para quienes monetizan sus producciones.  

De esta manera, cada clase solo implementa lo que realmente necesita.

## Explicación de Interfaces
En programación orientada a objetos, una **interfaz** define un contrato que una clase debe cumplir, sin imponer detalles de implementación.  
El ISP propone que esas interfaces sean **cohesivas y específicas**, reduciendo la obligación de implementar operaciones innecesarias.

## Estructura de Clases (UML)

![Diagrama ISP](/diagramas/01-diagrama-clases/01-solid-04-isp.png)  
[Ver diagrama en detalle](/diagramas/01-diagrama-clases/01-solid-04-isp.puml)

## Justificación Técnica
En el diagrama se observa que:

- `Productor` implementa `ISubirContenido` y `IMonetizarContenido`.  
- `Espectador` implementa `IReproducirContenido` y `IComentarContenido`.  
- `Administrador` implementa solo `ISubirContenido` (cuando modera contenido).  

De esta forma, **cada clase implementa únicamente lo que necesita**, eliminando dependencias innecesarias y mejorando la mantenibilidad del sistema.