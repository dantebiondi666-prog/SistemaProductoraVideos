|  |  |  |  |
|---|---|---|---|
| **Nombre de la Clase:** | PROYECTO | | |
| **Superclase:** | --- | | |
| **Subclase:** | ---| | |
| **Responsabilidades** | **Colaboradores** | **Pensamiento del objeto** | **Propiedad** |
|---|---|---|---|
| Registrar/actualizar datos del proyecto (crear/editar) | — | Necesito quedar identificado con datos mínimos válidos y guardados. | projectId, nombre, cliente, fechaInicioEstimada, fechaFinEstimada, estado, observaciones |
| Validar unicidad y coherencia de fechas | — | Verifico que mi nombre no duplique a otro y que inicio ≤ fin. | nombre, fechaInicioEstimada, fechaFinEstimada |
| Gestionar estado del proyecto (pendiente/en_curso/finalizado/pausado) | Etapa | Cambio mi estado respetando reglas; solo finalizo si todas mis etapas están finalizadas. | estado |
| Agregar/eliminar etapas del proyecto | Etapa | Incorporo o remuevo etapas que conforman mi plan de trabajo. | etapas (composición) |
| Registrar auditoría de creación/edición | — | Dejo evidencia de quién me creó/actualizó y cuándo. | creadoPor, creadoEn, actualizadoPor, actualizadoEn |
| Emitir notificación por creación/edición | ServicioNotificaciones, Notificacion | Al quedar guardado, aviso a los interesados por los canales definidos. | nombre, cliente, estado |
| Gestionar observaciones del proyecto | — | Mantengo un texto de observaciones generales asociado a mí. | observaciones |
| Calcular duración promedio de mis etapas | Etapa | Estimo un indicador de duración a partir de mis etapas. | — |
| Clonar desde un proyecto base (opcional) | Proyecto, Etapa | Puedo copiarme desde otro proyecto junto con su estructura de etapas. | nombre, cliente, fechas, estado, observaciones |