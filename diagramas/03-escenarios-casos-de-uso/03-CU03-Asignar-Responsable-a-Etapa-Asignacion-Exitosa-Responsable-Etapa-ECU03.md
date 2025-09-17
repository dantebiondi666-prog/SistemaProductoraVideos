| **Nombre del escenario:** |Asignacion Exitosa Responsable Etapa | | | |
|---|---|---|---|---|
| **Nombre del caso de uso:** | Asignar Responsable a Etapa | | **ID Única:** | ECU03 |
| **Área** | Gestión de Proyectos / Notificaciones | | | |
| **Actor(es):** | Coordinador | | | |
| **Descripción:** | Asignar un usuario como responsable de una etapa específica de un proyecto y notificarlo automáticamente. | | | |

| **Activar Evento:** | El Coordinador selecciona una etapa de un proyecto y elige la opción de "Asignar/Cambiar responsable". | **Identificadores e iniciadores de caso de uso** |
|---|---|---|
| **Tipo de señal:** | ☑️ Externa | ☐ Temporal | |

| **Pasos desempeñados (ruta principal)** | **Información para los pasos** |
|---|---|
| 1. Acceder a la etapa. | El actor navega al detalle de un proyecto y selecciona una etapa específica. |
| 2. Seleccionar la opción de asignación. | El sistema muestra la información de la etapa y el actor hace clic en la opción para asignar responsable. |
| 3. Elegir el responsable. | El sistema despliega un selector de usuarios y el actor elige al responsable. |
| 4. Validar y actualizar. | El sistema verifica que el usuario seleccionado sea válido y actualiza la etapa. |
| 5. Registrar el cambio. | El sistema registra quién, a quién y cuándo se realizó el cambio en el historial. |
| 6. Disparar notificación. | El sistema genera una notificación automática para el nuevo responsable. |

| **Condiciones, suposiciones y preguntas** | |
|---|---|
| **Precondiciones:** | Debe existir una etapa. El usuario a asignar debe ser un usuario válido del sistema. |
| **Poscondiciones:** | El responsable de la etapa es asignado, se ha enviado una notificación y el historial se actualizó. |
| **Suposiciones:** | Se asume que el sistema de notificaciones funciona y que los usuarios tienen un canal de contacto configurado. |
| **Reunir requerimientos:** | RF02 El sistema debe permitir agregar y modificar etapas en un proyecto, indicando responsable, estado (Pendiente, En curso, Finalizada), fechas estimadas y observaciones, RF04 El sistema debe enviar notificaciones automáticas (por mail y WhatsApp) al responsable, RF06  El sistema debe registrar automáticamente qué usuario completó cada tarea y los cambios de estado de cada etapa. |
| **Aspectos sobresalientes:** | Este caso de uso integra la gestión del proyecto con el sistema de notificaciones. |
| **Prioridad:** | Tiempo (Alta) |
| **Riesgo:** | Tiempo - Costo (Bajo) |