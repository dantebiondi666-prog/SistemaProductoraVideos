# Herencia

La herencia es un mecanismo del Diseño Orientado a Objetos que permite definir
relaciones de especialización entre clases, donde una clase hija hereda atributos
y comportamientos de una clase padre.

En el modelo de clases del sistema de la productora de videos no se utilizan
relaciones de herencia.

Esta decisión de diseño es intencional, ya que las clases del dominio representan
entidades conceptualmente diferentes (Proyecto, Etapa, Cliente, Usuario, Comentario,
Adjunto, HistorialEtapa), y no existen relaciones reales de tipo “es un” entre ellas.

Se prioriza el uso de composición y asociaciones para modelar las relaciones del
dominio, lo que permite un diseño más flexible y con menor acoplamiento.

Este enfoque evita jerarquías artificiales y se alinea con el principio de
Sustitución de Liskov (LSP), ya que solo debería utilizarse herencia cuando exista
una verdadera relación de especialización.

```md
## Ejemplo en el proyecto

En el modelo actual del sistema no se utiliza herencia entre las clases.

Las entidades del dominio (Proyecto, Etapa, Cliente, Usuario, Comentario, Adjunto e
HistorialEtapa) representan conceptos diferentes y no existe una relación de
especialización real entre ellas.

## Ejemplo de código (pseudocódigo)

```text
// No existe ejemplo de herencia en el proyecto,
// ya que el diseño no utiliza relaciones de tipo "es un".

Justificación técnica

La ausencia de herencia es una decisión de diseño. Aplicar herencia sin una verdadera relación de especialización generaría jerarquías artificiales.
De esta forma se respeta el principio de Sustitución de Liskov, utilizando herencia únicamente cuando existe una relación válida de tipo "es un".