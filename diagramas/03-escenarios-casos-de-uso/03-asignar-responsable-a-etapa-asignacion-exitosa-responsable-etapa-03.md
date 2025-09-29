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
| 1. Acceder a la etapa. | Vista `DetalleProyecto/{id}` con panel de Etapas; selección de Etapa{id, nombre, estado, responsableActual}. |
| 2. Seleccionar la opción de asignación. | Acción “Asignar/Cambiar responsable” disponible en la fila/menú de la etapa; apertura de modal/comando de asignación. |
| 3. Elegir el responsable. | Selector de usuarios activos (búsqueda por nombre/email; filtro por rol). Muestra lista `Usuario{ id, nombre, rol, activo }` y control Confirmar. |
| 4. Validar y actualizar. | Validaciones: usuario existe y activo; roles permitidos; evitar reasignación al mismo usuario; permisos del coordinador. Actualización de `Etapa.responsableId`. |
| 5. Registrar el cambio. | Auditoría: `EtapaHistorial{ etapaId, deUsuarioId, aUsuarioId, accion:"asignación", userId(actor), timestamp }`. |
| 6. Disparar notificación. | Notificación automática al nuevo responsable: componer mensaje (proyecto, etapa, enlace, asignador); invocar Servicio de Notificaciones (mail/WhatsApp) y almacenar resultado. |

| **Condiciones, suposiciones y preguntas** | |
|---|---|
| **Precondiciones:** | Debe existir una etapa. El usuario a asignar debe ser un usuario válido del sistema. |
| **Poscondiciones:** | El responsable de la etapa es asignado, se ha enviado una notificación y el historial se actualizó. |
| **Suposiciones:** | Se asume que el sistema de notificaciones funciona y que los usuarios tienen un canal de contacto configurado. |
| **Reunir requerimientos:** | RF02 El sistema debe permitir agregar y modificar etapas en un proyecto, indicando responsable, estado (Pendiente, En curso, Finalizada), fechas estimadas y observaciones, RF04 El sistema debe enviar notificaciones automáticas (por mail y WhatsApp) al responsable, RF06  El sistema debe registrar automáticamente qué usuario completó cada tarea y los cambios de estado de cada etapa. |
| **Aspectos sobresalientes:** | ¿Qué roles pueden ser responsables de etapa? ¿Se notifica también al responsable anterior cuando hay cambio? ¿Cuál es la plantilla? ¿Regla para evitar asignación redundante o forzar con justificación? ¿Qué hacer si el usuario no tiene canal configurado? ¿Restringir a usuarios activos del equipo? ¿Se permite asignar externos/cliente? Límites del selector: tamaño de búsqueda, paginación, validación de entrada. |
| **Prioridad:** | Tiempo (Alta) |
| **Riesgo:** | Tiempo - Costo (Bajo) |