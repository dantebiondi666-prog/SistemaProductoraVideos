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
| 1. Acceder al detalle del proyecto. | El actor navega y selecciona un proyecto de la lista para ver su detalle. |
| 2. Mostrar la lista de etapas. | El sistema muestra las etapas existentes del proyecto. |
| 3. Seleccionar una acción. | El actor elige si quiere agregar, editar o eliminar una etapa. |
| 4. Desplegar un formulario. | El sistema presenta un formulario para completar o modificar los datos de la etapa (nombre, responsable, fechas) |
| 5. Guardar los cambios. | El sistema valida y guarda los cambios. |
| 6. Confirmar la actualización. | El sistema actualiza la visualización de la lista de etapas y muestra un mensaje de éxito. |

| **Condiciones, suposiciones y preguntas** | |
|---|---|
| **Precondiciones:** | Debe existir un proyecto. El actor debe tener permisos de Coordinador. |
| **Poscondiciones:** | Las etapas del proyecto son actualizadas y se reflejan en el tablero de control. |
| **Suposiciones:** | Se asume que las etapas deben tener un responsable válido y fechas coherentes. |
| **Reunir requerimientos:** | RF02 - El sistema debe permitir la gestión (alta, baja, modificación) de etapas de un proyecto. |
| **Aspectos sobresalientes:** | La gestión de etapas está ligada a un proyecto existente. La validación de responsable y fechas es clave. |
| **Prioridad:** | Tiempo (Alta) |
| **Riesgo:** | Tiempo - Costo (Bajo) |