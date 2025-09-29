| **Nombre del escenario:** |Envio Notificacion Automatica Exitosa | | | |
|---|---|---|---|---|
| **Nombre del caso de uso:** | Enviar Notificaciones Automáticas | | **ID Única:** | ECU07 |
| **Área** |Notificaciones | | | |
| **Actor(es):** | Sistema de Notificaciones | | | |
| **Descripción:** | Enviar alertas automáticas a los responsables e interesados de proyectos y etapas ante eventos relevantes. | | | |

| **Activar Evento:** | Un evento específico ocurre en el sistema (ej., asignación de responsable o cambio de estado). | **Identificadores e iniciadores de caso de uso** |
|---|---|---|
| **Tipo de señal:** | ☑️ Externa | ☐ Temporal | |

| **Pasos desempeñados (ruta principal)** | **Información para los pasos** |
|---|---|
| 1. Ocurrencia del evento. | Evento detectado desde CU03/CU04 con contexto: proyecto, etapa, estado nuevo/anterior, actor que realizó la acción, timestamp; tipos de evento contemplados: asignación o cambio de responsable, cambio de estado de etapa (pendiente/en curso/finalizada). |
| 2. Identificar destinatarios. | Resolución de destinatarios según tipo de evento y reglas: responsable actual, coordinador y, si corresponde, cliente/administrador; evitar duplicados y excluir usuarios sin acceso. |
| 3. Determinar el canal. | Preferencias por usuario y política del sistema: envío por mail y WhatsApp si están configurados; si falta configuración, aplicar canal por defecto del sistema; registrar indisponibilidad de canal. |
| 4. Componer el mensaje. | Plantilla con variables: nombre de proyecto, etapa, nuevo estado o responsable asignado, enlaces a DetalleProyecto/DetalleEtapa, quién realizó la acción y fecha/hora; asunto y cuerpo normalizados. |
| 5. Enviar la notificación. | Integración con proveedor de notificaciones: envío por mail y por WhatsApp (dos llamadas si aplica); captura de respuesta del proveedor con estado, id de mensaje y latencia. |
| 6. Registrar el envío. | Registro en Historial/Notificaciones: {eventoId, destinatarios, canales usados, resultado por canal, reintentos, errorCode si aplica, timestamp, userId que provocó el evento}; programar reintento si el proveedor falla. |

| **Condiciones, suposiciones y preguntas** | |
|---|---|
| **Precondiciones:** | El usuario debe tener un canal de contacto definido. Debe haber conexión con el proveedor de notificaciones. |
| **Poscondiciones:** | La notificación se ha registrado y el destinatario está informado del evento. |
| **Suposiciones:** | Se asume que el sistema de notificaciones externo funciona correctamente. |
| **Reunir requerimientos:** | RF04:El sistema debe enviar notificaciones automáticas (por mail y WhatsApp), RF06.El sistema debe registrar automáticamente qué usuario completó cada tarea y los cambios de estado de cada etapa |
| **Aspectos sobresalientes:** | ¿Los envíos deben realizarse siempre por ambos canales o se respeta estrictamente la preferencia del usuario? ¿Cuál es la lista definitiva de eventos que disparan notificaciones y su mapeo de destinatarios? ¿Qué política de reintentos y ventana de tiempo se aplica ante fallos del proveedor? ¿Cuál es el canal por defecto cuando el usuario no tiene preferencias configuradas? ¿Qué contenido mínimo deben tener el asunto y el cuerpo del mensaje y qué enlaces deben incluir? ¿Cómo se evita notificar destinatarios duplicados o destinatarios sin permisos sobre el proyecto? ¿Existen límites de tasa de envío o reglas de silencio para evitar spam en cambios masivos? ¿Qué datos exactos deben persistirse en el historial para auditoría y trazabilidad?|
| **Prioridad:** | Tiempo (Media) |
| **Riesgo:** | Tiempo - Costo (Medio) |