# Polimorfismo

## Ejemplo en el proyecto

En el modelo actual del Sistema Productora de Videos no se aplica polimorfismo, ya que el diseño
no incorpora jerarquías de herencia ni interfaces que permitan tratar objetos de distintos tipos
a través de una abstracción común.

Todas las colaboraciones entre objetos se realizan mediante asociaciones directas entre clases
concretas del dominio.

Por este motivo, en esta versión del proyecto no se observa la aplicación del principio de
polimorfismo.

## Ejemplo de código

No se incluyen ejemplos de polimorfismo en el proyecto, debido a que el diseño actual no incorpora
herencia ni interfaces.


## Ejemplo de código (pseudocódigo)

objeto : ClaseBase

objeto = nueva SubclaseA()
objeto.ejecutar()

objeto = nueva SubclaseB()
objeto.ejecutar()

Justificación técnica

El polimorfismo se cumple porque el sistema trabaja con una referencia del tipo
ClaseBase y, sin conocer el tipo concreto de la instancia, invoca el mismo mensaje.
Cada subclase responde de manera distinta, respetando el contrato definido por la clase base, cumpliendo así el principio de sustitución de Liskov (LSP).