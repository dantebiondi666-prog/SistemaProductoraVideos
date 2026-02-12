# Polimorfismo

El polimorfismo es un principio del Diseño Orientado a Objetos que permite que
diferentes objetos puedan responder de manera distinta a un mismo mensaje,
a través de una abstracción común.

En el modelo actual del sistema de la productora de videos no se definen
jerarquías ni interfaces que permitan aplicar polimorfismo de forma explícita.

Sin embargo, el diseño se encuentra preparado para su incorporación en futuras
extensiones del sistema, ya que se basa en asociaciones y composición, evitando
dependencias rígidas entre las clases.

Por ejemplo, entidades como Adjunto o HistorialEtapa podrían evolucionar a
distintos tipos concretos sin necesidad de modificar las clases que los utilizan.

Este enfoque se alinea con los principios de Abierto/Cerrado (OCP) y de
Sustitución de Liskov (LSP), facilitando la extensión del comportamiento sin
afectar a los clientes existentes.

```md
## Ejemplo en el proyecto

En el modelo actual del sistema no se utiliza polimorfismo de manera explícita, ya que
no se definieron interfaces ni jerarquías de clases.

Sin embargo, el diseño está preparado para su incorporación futura mediante la
extensión de entidades como Adjunto o HistorialEtapa.

## Ejemplo de código (pseudocódigo)

```text
// No se implementa polimorfismo en el modelo actual.
// El diseño queda preparado para incorporar distintos tipos
// de adjuntos o registros de historial en el futuro.

Justificación técnica

El diseño basado en asociaciones y composición permite extender el comportamiento del sistema sin modificar las clases existentes.
Esto se alinea con el principio de Abierto/Cerrado, permitiendo incorporar
polimorfismo en futuras versiones del sistema.