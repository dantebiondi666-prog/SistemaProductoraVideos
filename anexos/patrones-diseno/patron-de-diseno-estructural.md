# Anexo - Aplicación de Patrón de Diseño Estructural - Facade

## Patrones de Diseño Estructural y su relación con SOLID

Los patrones de diseño estructural se enfocan en **cómo se organizan y relacionan las clases y objetos del sistema**, buscando **reducir el acoplamiento**, **simplificar la arquitectura** y **facilitar la extensibilidad**.

Se relacionan directamente con los principios **SOLID**, especialmente:

| Principio | Relación con Facade |
|-----------|---------------------|
| **SRP** (Responsabilidad Única) | Se encapsula la complejidad en una sola fachada. |
| **OCP** (Abierto/Cerrado) | Permite agregar nuevas funcionalidades sin modificar las clases internas. |
| **DIP** (Inversión de Dependencias) | El sistema depende de una interfaz simple, no de múltiples clases concretas. |

## Propósito y tipo del Patrón

**Propósito:**  
Reducir el acoplamiento entre los controladores / frontend y las clases internas del sistema (`Proyecto`, `Etapa`, `Notificacion`, `AuditoriaProyecto`, `ServicioNotificaciones`, etc.).  
Actualmente, para realizar una sola acción se debe interactuar con múltiples clases, lo que genera **complejidad**, **duplicación de lógica** y dificulta la **escalabilidad**.

**Tipo:**  
El patrón seleccionado es **Facade**, porque ofrece **una interfaz unificada y simplificada** para operaciones complejas, ocultando la lógica interna del sistema sin modificar las clases existentes.

---

## Motivación

### Problema detectado

Actualmente, al realizar una acción concreta (por ejemplo, *crear un proyecto con etapas y notificar al responsable*), se requiere llamar a distintas clases y coordinar varias tareas manualmente:

- `Proyecto` gestiona lógica de negocio.  
- `Etapa` tiene cambios de estado.  
- `HistorialEtapa` y `AuditoriaProyecto` registran logs.  
- `ServicioNotificaciones` envía mensajes.  
- `Notificacion` debe generarse en base a eventos.  

Esto genera **acoplamiento excesivo** y **dificultad para mantener o escalar el sistema**, ya que cualquier cambio obliga a modificar múltiples clases.

### Solución propuesta con Facade

Se incorpora la clase `SistemaProductoraFacade`, que actúa como **punto único de acceso**, centralizando la lógica compleja:

✔ Simplifica el uso desde controladores y API REST.  
✔ Reduce el acoplamiento entre componentes.  
✔ Permite agregar nuevas funciones sin modificar clases internas.  
✔ Organiza pasos complejos en un flujo claro y mantenible.

## Estructura de Clases

![Diagrama Facade](/diagramas/01-diagrama-clases/01-patron-estructural-facade.png)

## Justificación Técnica de la Estructura de Clases

### Detalle de la clase SistemaProductoraFacade

La fachada se modela como una **clase de aplicación** que coordina varios servicios internos del sistema. No representa una entidad de dominio, sino un **objeto de alto nivel** que orquesta colaboraciones entre otros objetos.

**Atributos (dependencias internas)**

SistemaProductoraFacade mantiene referencias privadas a servicios concretos:

*   servicioProyectos : ServicioProyectosServicio de aplicación que encapsula la lógica para crear, editar y consultar objetos concretos Proyecto.
    
*   servicioEtapas : ServicioEtapasGestiona el ciclo de vida de las instancias Etapa (altas, bajas, cambios de estado, asignación de responsables).
    
*   servicioNotificaciones : ServicioNotificacionesImplementa el envío real de notificaciones (Email, WhatsApp, Slack) a partir de objetos Notificacion.
    
*   servicioAuditoria : ServicioAuditoriaUtiliza la clase de dominio AuditoriaProyecto para registrar eventos relevantes sobre proyectos (creación, edición, etc.).
    
*   servicioReportes : ServicioReportesCalcula métricas (MetricasProyecto, MetricasEtapas) y construye objetos Adjunto para la exportación de reportes (PDF/CSV).
    

Estas dependencias se **inyectan en el constructor** de la fachada (inyección de dependencias). De esta forma, SistemaProductoraFacade **depende de abstracciones** y no instancia directamente los servicios, respetando DIP.

### ✔ Clases incluidas y su rol

| Clase | Rol dentro del patrón |
|------|------------------------|
| **SistemaProductoraFacade** | Es la fachada. Centraliza la lógica compleja del sistema y ofrece un punto único de acceso para operaciones recurrentes. Reduce el acoplamiento y simplifica el uso del sistema desde controladores o APIs externas. |
| **Proyecto** | Entidad principal del dominio. La fachada delega la creación, modificación y obtención de datos a esta clase. |
| **Etapa** | Responsable de la gestión del estado y cambios de responsables. La fachada simplifica su uso y encapsula la lógica asociada a etapas. |
| **AuditoriaProyecto** | Lleva registro de acciones sobre proyectos. La fachada lo utiliza para registrar logs sin que el controlador deba conocer su implementación interna. |
| **HistorialEtapa** | Registra eventos o cambios sobre una etapa. Se invoca desde la fachada para mantener el registro histórico del proyecto. |
| **Notificacion** | Se genera cuando ocurre un evento relevante. La fachada coordina su creación. |
| **ServicioNotificaciones** | Es el servicio concreto que envía notificaciones (WhatsApp, Email, Slack, etc.). La fachada lo usa para enviar sin que el resto del sistema conozca su funcionamiento interno. |
| **Adjunto** | Permite generar y adjuntar archivos (reportes PDF, CSV, etc.). La fachada unifica su creación desde una interfaz simple. |


---

✔ Flujo estructural: cómo resuelve el problema real del sistema
---------------------------------------------------------------

1.  Un **controlador** o endpoint REST tiene una **dependencia directa** únicamente hacia SistemaProductoraFacade. La instancia de fachada se construye inyectando ServicioProyectos, ServicioEtapas, ServicioAuditoria, ServicioReportes y ServicioNotificaciones.
    
2.  Cada vez que el usuario dispara un caso de uso (crear proyecto, cambiar estado, generar reporte), el controlador **invoca un método público de la fachada** en lugar de llamar a varias clases por separado.
    
3.  Internamente, la fachada **instancia y coordina objetos concretos** (Proyecto, Etapa, Notificacion, Adjunto) a través de sus servicios. Así, las relaciones entre clases del dominio quedan encapsuladas dentro de la fachada y de los servicios.
    
4.  Si en el futuro cambia la implementación de alguno de esos servicios (por ejemplo, un nuevo proveedor de notificaciones o un motor distinto de reportes), solo se ajustan las clases internas. La interfaz de SistemaProductoraFacade y el código de los controladores permanecen sin cambios.

### ✔ Beneficios logrados

- **Disminución del acoplamiento** entre capas externas y lógica interna.
- **Centralización de flujos complejos** → facilita el mantenimiento.
- **Fácil extensión**: agregar pasos nuevos solo requiere modificar la fachada.
- **Mayor limpieza en controladores** → se evita lógica duplicada.
- **Escalabilidad**: si cambia una tecnología interna (notificaciones, auditorías, adjuntos), no se rompe el resto del sistema.

---

### 📌 Conclusión técnica

El uso del patrón **Facade** **reorganiza la arquitectura**, otorgando una **interfaz única y simple**, que oculta la complejidad del dominio. De este modo, la fachada se convierte en una capa intermedia entre los controladores y las clases del dominio, promoviendo una arquitectura más **mantenible, extensible y alineada con SOLID**.