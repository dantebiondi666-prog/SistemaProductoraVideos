# Herencia

## Ejemplo en el proyecto

En el proyecto se aplica herencia en el manejo de los estados de una etapa.  
La clase abstracta `EstadoEtapa` define el comportamiento común de los estados, y las clases
`Pendiente`, `EnCurso`, `Bloqueada` y `Finalizada` heredan de ella, especializando el
comportamiento según el estado concreto.

Este diseño permite modelar los distintos estados de una etapa como objetos con un
comportamiento específico, reutilizando la estructura definida en la clase base.

## Fragmento de diagrama UML

```plantuml
@startuml

abstract class EstadoEtapa {
  + cambiarEstado(e: Etapa, nuevo: EstadoEtapa)
  + agregarComentario(e: Etapa)
}

class Pendiente
class EnCurso
class Bloqueada
class Finalizada

EstadoEtapa <|-- Pendiente
EstadoEtapa <|-- EnCurso
EstadoEtapa <|-- Bloqueada
EstadoEtapa <|-- Finalizada

@enduml
```

## Ejemplo de código (pseudocódigo)

```text
estado = new Pendiente()
estado.cambiarEstado(etapa, new EnCurso())
```
```md
### Justificación técnica

En el proyecto se aplica herencia en el manejo de los estados de una etapa.  
La clase abstracta `EstadoEtapa` define el comportamiento común de los estados, y las clases `Pendiente`, `EnCurso`, `Bloqueada` y `Finalizada` heredan de ella, especializando el comportamiento según el estado concreto.