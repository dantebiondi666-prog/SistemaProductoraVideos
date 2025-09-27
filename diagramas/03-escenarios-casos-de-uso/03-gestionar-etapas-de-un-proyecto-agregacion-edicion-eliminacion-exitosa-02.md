| **Nombre del escenario:** |Agregación, Edición o Eliminación Etapa Exitosa | | | |
|---|---|---|---|---|
| **Nombre del caso de uso:** | Gestionar Etapas de un Proyecto | | **ID Única:** | ECU02 |
| **Área** | Gestión de Proyectos | | | |
| **Actor(es):** | Coordinador | | | |
| **Descripción:** | Permite a un Coordinador agregar, editar o eliminar etapas de un proyecto para su seguimiento. | | | |

| **Activar Evento:** | El Coordinador abre el detalle de un proyecto y selecciona la opción "Gestionar etapas". | **Identificadores e iniciadores de caso de uso** |
|---|---|---|
| **Tipo de señal:** | ☑️ Externa | ☐ Temporal | |

| **Pasos desempeñados (ruta principal)** | **Información para los pasos** |
|---|---|
| 1. Acceder al detalle del proyecto. | Vista `DetalleProyecto/{id}` abierta desde la lista; contexto del proyecto cargado (id, nombre, cliente, estado). |
| 2. Mostrar la lista de etapas. | Lista/tabla de etapas del proyecto con columnas: nombre, responsable, estado, fechas inicio/fin estimadas, observaciones. Estado vacío: mensaje “sin etapas”. |
| 3. Seleccionar una acción. | Acciones disponibles sobre la lista: Agregar, Editar (fila seleccionada) y Eliminar (fila seleccionada) con diálogo de confirmación para eliminar. |
| 4. Desplegar un formulario. | Formulario EtapaForm (alta/edición) con campos: nombre, responsable (selector de usuario activo), estado (pendiente/en curso/finalizada), fechas inicio/fin estimadas, observaciones. Controles: Guardar / Cancelar. En edición, campos pre-completados. |
| 5. Guardar los cambios. | Validaciones y persistencia: responsable existente/activo; coherencia de fechas; estado permitido. Al confirmar: crear/actualizar/eliminar registro `Etapa{...}` y registrar auditoría (acción, usuario, timestamp). |
| 6. Confirmar la actualización. | Actualización y confirmación: refresco de la lista de etapas con el cambio aplicado; mensaje de éxito (toast/snackbar). |

| **Condiciones, suposiciones y preguntas** | |
|---|---|
| **Precondiciones:** | Debe existir un proyecto. El actor debe tener permisos de Coordinador. |
| **Poscondiciones:** | Las etapas del proyecto son actualizadas y se reflejan en el tablero de control. |
| **Suposiciones:** | Se asume que las etapas deben tener un responsable válido y fechas coherentes. |
| **Reunir requerimientos:** | RF02 - El sistema debe permitir la gestión (alta, baja, modificación) de etapas de un proyecto. |
| **Aspectos sobresalientes:** | ¿Estados válidos exactos de la etapa (pendiente/en curso/finalizada) y reglas de transición? ¿Se permite eliminar una etapa con trabajo asociado o debe quedar en estado “anulada”? Criterio de responsable válido: ¿solo usuarios activos del equipo? ¿roles permitidos? Regla de fechas: ¿obligatorias ambas? ¿zona horaria y formato ¿Se debe notificar al responsable cuando se crea/edita/elimina una etapa? ¿Campos obligatorios exactos y límites? |
| **Prioridad:** | Tiempo (Alta) |
| **Riesgo:** | Tiempo - Costo (Bajo) |