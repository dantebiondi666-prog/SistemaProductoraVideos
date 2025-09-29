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
| 1. Acceder a la etapa. | Vista DetalleEtapa/{id} con contexto de la etapa cargado: id, nombre, estadoActual, responsable, fechas estimadas y cantidad de tareas pendientes. |
| 2. Seleccionar nuevo estado. | Control de selección de estado con opciones válidas para el flujo: Pendiente, En curso, Finalizada; se muestran reglas visibles de transición según el estadoActual. |
| 3. Validar reglas de negocio. | Verificación de permisos del actor; chequeo de transición permitida; restricción “no finalizar si hay tareas pendientes”; coherencia de fechas (asignar fechaInicio al pasar a En curso si no existe). |
| 4. Actualizar estado. | Actualización de Etapa.estado; si pasa a En curso registrar fechaInicio; si pasa a Finalizada registrar fechaFin; guardar usuario que realizó el cambio. |
| 5. Registrar el cambio. | Creación de registro en Historial: {etapaId, estadoAnterior, estadoNuevo, userId, timestamp, observación opcional}; enlazar con proyecto para trazabilidad. |
| 6. Enviar notificación. | Composición del mensaje con proyecto, etapa y nuevo estado; destinatarios según iniciador: si cambia el Responsable notificar al Coordinador, si cambia el Coordinador notificar al Responsable; envío vía Servicio de Notificaciones (mail/WhatsApp) y almacenamiento del resultado. |

| **Condiciones, suposiciones y preguntas** | |
|---|---|
| **Precondiciones:** | La etapa debe existir y el actor debe tener los permisos para realizar el cambio. |
| **Poscondiciones:** | El estado de la etapa se actualiza, el historial de la etapa se registra y se envían las notificaciones correspondientes. |
| **Suposiciones:** | Se asume que las reglas de negocio están claramente definidas en el sistema. |
| **Reunir requerimientos:** | RF02 El sistema debe permitir agregar y modificar etapas en un proyecto, indicando responsable, estado (Pendiente, En curso, Finalizada), fechas estimadas y observaciones, RF04 Notificaciones automáticas: El sistema debe enviar notificaciones automáticas (por mail y WhatsApp) al responsable, RF06  El sistema debe registrar automáticamente qué usuario completó cada tarea y los cambios de estado de cada etapa.. |
| **Aspectos sobresalientes:** | ¿Cuáles son los estados válidos y las transiciones permitidas para la etapa? ¿Cómo se comprueba la regla “no finalizar si hay tareas pendientes” y qué fuente de datos se usa para el conteo? ¿Al pasar a En curso se debe registrar obligatoriamente fechaInicio? ¿Y al pasar a Finalizada, fechaFin? ¿Quiénes son los destinatarios de la notificación según quién inicia el cambio y cuál es la plantilla del mensaje? ¿Se admite revertir estados? ¿En qué condiciones y con qué permisos?|
| **Prioridad:** | Tiempo (Alta) |
| **Riesgo:** | Tiempo - Costo (Medio) |