# Anexo – Aplicación de Patrón de Diseño de Comportamiento – Strategy

## Patrones de Diseño de Comportamiento y su relación con SOLID
Los patrones de comportamiento organizan el cómo colaboran los objetos, mejorando la flexibilidad y mantenibilidad.
Existen distintos patrones de comportamiento como:

- Command: Encapsular una acción como objeto independiente.
- State: Cambiar el comportamiento de un objeto según su estado interno
- Template Method: Definir la estructura de un proceso dejando pasos personalizables
- Chain of Responsibility: Pasar una solicitud por una cadena de manejadores hasta que uno la procese
- Strategy: Cambiar cómo se ejecuta una acción o algoritmo
- Observer: Reaccionar cuando ocurre un evento


En particular, Strategy que es el patrón elegido para trabajar favorece:

- **SRP (Responsabilidad Única):** el algoritmo se encapsula en una estrategia concreta; el contexto (`Etapa`) deja de mezclar reglas heterogéneas.
- **OCP (Abierto/Cerrado):** nuevas variantes de cálculo se agregan como nuevas estrategias sin modificar el contexto.
- **LSP (Sustitución de Liskov):** cualquier `IEstimadorDuracion` debe poder reemplazarse por otro sin romper el sistema.
- **ISP (Segregación de Interfaces):** la interfaz de estrategia es mínima (`estimarDuracion(etapa)`).
- **DIP (Inversión de Dependencias):** `Etapa` depende de la abstracción `IEstimadorDuracion`, no de implementaciones concretas.

---

## Propósito y Tipo del Patrón
**Propósito:** desacoplar el algoritmo de estimación de duración/fecha fin de una `Etapa` para permitir múltiples métodos (por tareas, por story points, por históricos, por criticidad/SLA) sin introducir estructuras condicionales extensas.

**Tipo:** **Strategy**(patrón de comportamiento). 
Motivos de elección:
Se eligió el patrón Strategy porque el sistema presenta una alta variabilidad en los algoritmos de estimación, que dependen del tipo de proyecto, su nivel de criticidad y la disponibilidad de datos. Esta estructura permite extender o reemplazar fácilmente el método de estimación sin modificar la clase Etapa, en coherencia con los principios de abierto/cerrado (OCP) e inversión de dependencias (DIP). Además, favorece la testabilidad del sistema y posibilita una configuración dinámica según el entorno o las necesidades específicas de cada cliente.

---

## Motivación
**Situación original (problema):**
- La clase `Etapa` debía calcular `fechaFinEstimada`, pero los criterios varían según el tipo de proyecto::
  - En proyectos ágiles: **story points/velocidad** del equipo.
  - En proyectos con desglose detallado de tareas: **tiempos por tarea**.
  - En campañas urgentes: **SLA por criticidad**.
  - En escenarios con poca data actual: **promedios históricos**.
- Esto obligaba a `Etapa` a concentrar múltiples condiciones (`if/else`) y lógica cambiante, afectando la claridad, la mantenibilidad, las pruebas y la escalabilidad.

**Limitaciones:**
- Dificultad para **agregar** nuevas variantes (p. ej. estimador externo o basado en IA).
- Alto acoplamiento entre **la lógica de dominio** y **fuentes de datos**  utilizadas para la estimación.
- Duplicación de lógica y riesgos de **regresiones** al editar código central.

**Solución con Strategy (qué cambia):**
- Se introduce la interfaz `IEstimadorDuracion` con un único contrato:  
  `estimarDuracion(etapa: Etapa): Duration`.
- `Etapa` actúa como **Contexto** y delegando el cálculo a una estrategia concreta inyectada.
- Las estrategias concretas implementan distintos métodos de estimación:
  - `EstimadorPorTareas` (usa `TareasRepository` para sumar horas pendientes).
  - `EstimadorPorStoryPoints` (utiliza la **velocidad** del equipo).
  - `EstimadorPorHistoricos` (consulta `HistoricosRepository`).
  - `EstimadorPorCriticidad` (consulta `SlaRepository` según el nivel de servicio ).
- La **selección** de estrategia se resuelve fuera del dominio (composición raíz o un **Factory** de aplicación) según reglas de negocio (tipo de proyecto, criticidad, disponibilidad de datos).

**Resultado:**
- `Etapa` queda simple, enfocada en su **estado/identidad**.
- Los algoritmos se **agregan/cambian** sin tocar el modelo central.
- Mejora la **testabilidad**: cada estrategia se prueba en aislamiento con dobles de test.

---

## Estructura de Clases
Solo se muestran clases del patrón:

- `Etapa` (**Contexto**): mantiene referencia a `IEstimadorDuracion` y delega el cálculo en `recalcularFechaFin()`.
- `IEstimadorDuracion` (**Strategy**): define la interfaz con el método `estimarDuracion(etapa)`.
- `EstimadorPorTareas`, `EstimadorPorStoryPoints`, `EstimadorPorHistoricos`, `EstimadorPorCriticidad` (**ConcreteStrategies**): implementan distintas formas de estimación.
- Repositorios auxiliares `TareasRepository`, `HistoricosRepository`, `SlaRepository`, proporcionan datos de soporte a cada estrategia concreta.

**Ver diagrama en tamaño completo:**  
![01-patron-comportamiento-strategy.png](/diagramas/01-diagrama-clases/01-patron-comportamiento-strategy.png)


---

## Justificación Técnica de la Estructura de Clases

### Clases y responsabilidades
- **Etapa (Contexto):**
  - *Responsabilidad:* permite orquestar el cálculo sin conocer el algoritmo.
  - *Interacción:* mantiene una instancia activa `estimador: IEstimadorDuracion` y la invoca en `recalcularFechaFin()`; puede reconfigurarse vía `configurarEstimador(...)`
  - *Indispensable porque:* centraliza el punto de uso donde se necesita el resultado, pero delega el **cómo**.

- **IEstimadorDuracion (Strategy):**
  - *Responsabilidad:* contrato único y estable para todas las variantes de estimación.
  - *Interacción:* define el método que `Etapa` invoca.
  - *Indispensable porque:* permite **sustitución** (LSP) transparente y **extensibilidad**(OCP/DIP)

- **EstimadorPorTareas (ConcreteStrategy):**
  - *Responsabilidad:* calcular duración sumando horas pendientes de tareas.
  - *Interacción:* usa `TareasRepository` para datos; devuelve `Duration`.
  - *Indispensable porque:* es necesaria porque maneja proyectos donde se dispone de un desglose detallado de tareas.

- **EstimadorPorStoryPoints (ConcreteStrategy):**
  - *Responsabilidad:* traducir SP a horas según la **velocidad** del equipo.
  - *Interacción:* puede recibir `velocidadEquipo` por configuración, es decir en lugar de que la propia clase busque o calcule esa información se lo entrega desde afuera usando configuración de dependencias.
  - *Indispensable porque:* cubre proyectos ágiles.

- **EstimadorPorHistoricos (ConcreteStrategy):**
  - *Responsabilidad:* estimar por promedios históricos (tipo/complejidad).
  - *Interacción:* consulta `HistoricosRepository`.
  - *Indispensable porque:* es utilizado mayormente cuando hay pocos datos actuales.

- **EstimadorPorCriticidad (ConcreteStrategy):**
  - *Responsabilidad:* calcular la duración estimada en función de los acuerdos de nivel de servicio (SLA) según la criticidad de la etapa.
  - *Interacción:* consulta `SlaRepository` para obtener los tiempos establecidos.
  - *Indispensable porque:* permite cubrir escenarios urgentes o con compromisos formales de servicio.

- **Repositorios** (`TareasRepository`, `HistoricosRepository`, `SlaRepository`):
  - *Responsabilidad:* acceso a fuentes de datos específicas.
  - *Interacción:* inyectados en cada estrategia que los requiere.
  - *Indispensable porque:* separan **infraestructura** de **dominio/algoritmo**.

### Flujo del comportamiento (paso a paso)
Cuando se crea o actualiza una Etapa (contexto), el sistema selecciona e inyecta una implementación de IEstimadorDuracion (estrategia concreta) según las reglas de negocio. Luego, al requerir el recálculo, Etapa colabora con la estrategia: le solicita la duración (estimarDuracion(this)) y con ese resultado proyecta fechaFinEstimada sumándolo a fechaInicioEstimada. Si cambian las condiciones (p. ej., de históricos a tareas), se reconfigura la estrategia y se vuelve a recalcular, sin modificar la clase Etapa. Cuando aplica, las estrategias a su vez colaboran con sus repositorios de soporte (TareasRepository, HistoricosRepository, SlaRepository) para obtener datos necesarios.

### Por qué esta estructura es adecuada
Esta estructura resulta adecuada porque maximiza la cohesión y minimiza el acoplamiento, asegurando que cada clase tenga una única responsabilidad bien definida. Además, cumple con los principios de abierto/cerrado (OCP) y de inversión de dependencias (DIP), ya que permite agregar nuevas estrategias sin necesidad de modificar la clase Etapa. Favorece asimismo la realización de pruebas unitarias, facilitando la verificación independiente de cada componente. Finalmente, posibilita la implementación de políticas de selección de estrategias sin afectar la lógica del dominio, gracias al uso de dependencias, lo que contribuye a un diseño más flexible y mantenible.