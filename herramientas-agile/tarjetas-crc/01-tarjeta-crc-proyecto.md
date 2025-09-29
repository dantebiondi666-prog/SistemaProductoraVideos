|  |  |  |  |
|---|---|---|---|
| **Nombre de la Clase:** | PROYECTO | | |
| **Superclase:** | --- | | |
| **Subclase:** | ---| | |
| **Responsabilidades** | **Colaboradores** | **Pensamiento del objeto** | **Propiedad** |
|---|---|---|---|
| Registrar/actualizar mis datos (crear/editar) | — | Necesito quedar creado/actualizado con mis datos mínimos para estar disponible para seguimiento. | nombre, cliente, fechaInicioEstimada, fechaFinEstimada, estado, observaciones |
| Validar unicidad de nombre | — | Conozco mi nombre y debo asegurar que no duplique a otro (según normalización acordada). | nombre |
| Validar coherencia de fechas (inicio ≤ fin) | — | Conozco mi fecha de inicio estimada y mi fecha de fin estimada y verifico que inicio ≤ fin. | fechaInicioEstimada, fechaFinEstimada |
| Gestionar mi estado (pendiente/en_curso/finalizado/pausado) | Etapa | Actualizo mi estado respetando reglas; solo me finalizo si todas mis etapas están finalizadas. | estado |
| Agregar una etapa a mi estructura de trabajo | Etapa | Incorporo una etapa a mi plan de trabajo cuando es necesario. | — (relación con Etapa, composición) |
| Eliminar una etapa de mi estructura de trabajo | Etapa | Remuevo una etapa cuando deja de corresponder. | — (relación con Etapa, composición) |
| Registrar/actualizar mis observaciones | — | Mantengo observaciones generales asociadas a mí. | observaciones |
| Calcular la duración promedio de mis etapas | Etapa | Calculo una métrica a partir de las fechas de mis etapas para análisis. | — (usa relación con Etapa: fechas) |
| Finalizarme si corresponde | Etapa | Me doy por finalizado cuando todas mis etapas están finalizadas. | estado |
| Emitir notificación al crear/editar | ServicioNotificaciones, Notificacion | Cuando quedo creado o editado, disparo un aviso a los interesados. | nombre, cliente, estado |