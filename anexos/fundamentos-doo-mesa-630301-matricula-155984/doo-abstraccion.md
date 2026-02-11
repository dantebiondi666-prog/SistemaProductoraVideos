# Abstracción

La abstracción en el diseño orientado a objetos consiste en identificar las entidades relevantes del dominio del problema y modelarlas como clases, ocultando los detalles innecesarios y exponiendo únicamente la información y el comportamiento que resultan significativos para el sistema.

En el sistema de gestión de una productora de videos, la abstracción permite representar conceptos del mundo real como proyectos y etapas de trabajo mediante objetos que encapsulan tanto datos como operaciones asociadas.

Desde el punto de vista de los principios SOLID, la abstracción se relaciona principalmente con el principio de inversión de dependencias (DIP), ya que promueve que los módulos de alto nivel trabajen sobre modelos conceptuales estables y no sobre detalles concretos de implementación. Asimismo, favorece el principio de responsabilidad única (SRP), al delimitar claramente el propósito de cada clase.

En relación con los patrones de diseño, la abstracción es un habilitador fundamental para patrones como Strategy y Factory Method, ya que estos se basan en la definición de comportamientos y responsabilidades a nivel conceptual.

---

## Ejemplo en el proyecto

En el proyecto se abstraen conceptos centrales del dominio a través de las clases `Proyecto` y `Etapa`, las cuales representan un proyecto audiovisual y una etapa de producción respectivamente.

Estas clases forman parte del diagrama definido en el archivo:

- `01-diagrama-clases-final.puml`

En dicho diagrama puede observarse cómo un `Proyecto` se relaciona con múltiples instancias de `Etapa`, modelando la estructura real de un proyecto compuesto por varias etapas.

Esta selección de clases refleja la abstracción del dominio, ya que se omiten detalles técnicos de implementación y se representan únicamente los conceptos relevantes para la gestión de la productora.

Justificación técnica:

- La clase `Proyecto` encapsula la información y los comportamientos propios de un proyecto audiovisual.
- La clase `Etapa` encapsula la información y los comportamientos asociados a una fase del proyecto.
- La relación entre ambas clases permite representar una composición lógica del dominio.

---

## Ejemplo de código (pseudocódigo)

```text
clase Proyecto
    atributo nombre
    atributo estado
    atributo cliente
    lista de Etapa etapas

    método agregarEtapa(etapa)
        agregar etapa a la lista
fin clase
