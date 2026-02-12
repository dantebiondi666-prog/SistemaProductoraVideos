# Herencia

### Ejemplo en el proyecto

En el diagrama de clases final del sistema no se modelaron relaciones de herencia
entre las clases del dominio.

Las entidades principales del sistema (Proyecto, Etapa, Usuario, Cliente, Comentario,
Adjunto, Notificacion, HistorialEtapa y AuditoriaProyecto) representan conceptos
diferentes y no existe entre ellas una relación de especialización de tipo “es un”.

### Fragmento de diagrama UML

```plantuml
class Proyecto
class Etapa
class Usuario
class Adjunto

Proyecto "1" *-- "1..*" Etapa
Etapa "1" o-- "0..*" Adjunto

## Ejemplo de código (pseudocódigo)

proyecto = obtenerProyecto(id)

etapas = proyecto.listarEtapas()

Justificación técnica

En este diseño no se aplica herencia debido a que no se identificaron jerarquías
conceptuales válidas dentro del dominio del problema.

El principio de sustitución de Liskov (LSP) se cumple de forma vacía, ya que al no
existir relaciones de herencia, no existen subclases que deban respetar contratos de
una clase base.

Esta decisión evita jerarquías artificiales y mantiene un bajo acoplamiento entre las
clases.

Respecto a los patrones de diseño, en el modelo actual no se introducen patrones
basados en herencia, ya que el objetivo principal del trabajo fue modelar el dominio
y los casos de uso, priorizando claridad y simplicidad en el diseño.