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
