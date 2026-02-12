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

