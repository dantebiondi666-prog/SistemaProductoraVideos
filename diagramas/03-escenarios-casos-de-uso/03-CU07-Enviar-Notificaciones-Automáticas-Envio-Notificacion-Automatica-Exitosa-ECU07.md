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
| 1. Ocurrencia del evento. | Un cambio en el sistema (ej., CU03 o CU04) dispara el proceso de notificación. |
| 2. Identificar destinatarios. |  El sistema identifica a los usuarios que deben ser notificados. |
| 3. Determinar el canal. | Se revisa la configuración del usuario para determinar el canal (correo, WhatsApp, etc.). |
| 4. Componer el mensaje. | El sistema crea un mensaje con información clara del proyecto y el evento. |
| 5. Enviar la notificación. | La notificación es enviada al proveedor de notificaciones. |
| 6. Registrar el envío. | Se registra en el historial del sistema el intento o el éxito del envío. |

| **Condiciones, suposiciones y preguntas** | |
|---|---|
| **Precondiciones:** | El usuario debe tener un canal de contacto definido. Debe haber conexión con el proveedor de notificaciones. |
| **Poscondiciones:** | La notificación se ha registrado y el destinatario está informado del evento. |
| **Suposiciones:** | Se asume que el sistema de notificaciones externo funciona correctamente. |
| **Reunir requerimientos:** | RF04:El sistema debe enviar notificaciones automáticas (por mail y WhatsApp), RF06.El sistema debe registrar automáticamente qué usuario completó cada tarea y los cambios de estado de cada etapa |
| **Aspectos sobresalientes:** | Es un caso de uso de sistema, no iniciado por un actor humano. El reintento en caso de fallo es un aspecto clave. |
| **Prioridad:** | Tiempo (Media) |
| **Riesgo:** | Tiempo - Costo (Medio) |