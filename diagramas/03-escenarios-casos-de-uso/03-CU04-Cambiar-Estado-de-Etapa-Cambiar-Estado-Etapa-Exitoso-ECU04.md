| **Nombre del escenario:** |Cambiar Estado Etapa Exitoso | | | |
|---|---|---|---|---|
| **Nombre del caso de uso:** | Cambiar Estado de Etapa | | **ID Única:** | ECU04 |
| **Área** | Gestión de Proyectos / Notificaciones | | | |
| **Actor(es):** | Responsable de la etapa / Coordinador | | | |
| **Descripción:** | Permite a los actores cambiar el estado de una etapa de acuerdo con reglas de negocio preestablecidas. | | | |

| **Activar Evento:** | El actor accede al detalle de una etapa y selecciona la opción para cambiar el estado. | **Identificadores e iniciadores de caso de uso** |
|---|---|---|
| **Tipo de señal:** | ☑️ Externa | ☐ Temporal | |

| **Pasos desempeñados (ruta principal)** | **Información para los pasos** |
|---|---|
| 1. Acceder a la etapa. | El actor navega al detalle de una etapa. |
| 2. Seleccionar nuevo estado. | El sistema muestra el estado actual y las opciones disponibles (Pendiente, En curso, Finalizada). El actor elige una. |
| 3. Validar reglas de negocio. | El sistema verifica que el cambio sea válido (ej., no se puede finalizar si hay tareas pendientes). |
| 4. Actualizar estado. | Si la validación es exitosa, el sistema actualiza el estado en la base de datos. |
| 5. Registrar el cambio. | El sistema registra el evento y la información del cambio en el historial. |
| 6. Enviar notificación. | Se envían notificaciones a los interesados (ej., el coordinador y el responsable). |

| **Condiciones, suposiciones y preguntas** | |
|---|---|
| **Precondiciones:** | La etapa debe existir y el actor debe tener los permisos para realizar el cambio. |
| **Poscondiciones:** | El estado de la etapa se actualiza, el historial de la etapa se registra y se envían las notificaciones correspondientes. |
| **Suposiciones:** | Se asume que las reglas de negocio están claramente definidas en el sistema. |
| **Reunir requerimientos:** | RF02 El sistema debe permitir agregar y modificar etapas en un proyecto, indicando responsable, estado (Pendiente, En curso, Finalizada), fechas estimadas y observaciones, RF04 Notificaciones automáticas: El sistema debe enviar notificaciones automáticas (por mail y WhatsApp) al responsable, RF06  El sistema debe registrar automáticamente qué usuario completó cada tarea y los cambios de estado de cada etapa.. |
| **Aspectos sobresalientes:** | La validación de las reglas de negocio es crítica para la integridad de los datos. |
| **Prioridad:** | Tiempo (Alta) |
| **Riesgo:** | Tiempo - Costo (Medio) |