|  |  |  |  |
|---|---|---|---|
| Nombre de la Clase: | NOTIFICACION | | |
| Superclase: | — | | |
| Subclases: | — | | |
| **Responsabilidades** | **Colaboradores** | **Pensamiento del objeto** | **Propiedad** |
|---|---|---|---|
| Definir el canal de envío del aviso | Usuario | Selecciono el medio adecuado según la preferencia del destinatario o la política del evento. | canal |
| Componer asunto y mensaje del aviso | Etapa | Necesito comunicar con claridad qué pasó y en qué etapa/proyecto ocurrió. | asunto, mensaje |
| Direccionar el aviso al destinatario correcto | Usuario | Debo llegar a la persona indicada para que actúe a tiempo. | destino |
| Programar el envío (cuando corresponde) | ServicioNotificaciones | Puedo quedar pendiente para enviarme en el momento configurado. | fechaHora, canal |
| Solicitar el envío al servicio de notificaciones | ServicioNotificaciones | Delego el transporte del mensaje al proveedor configurado. | canal |
| Registrar el resultado del envío | ServicioNotificaciones | Dejo trazabilidad del éxito o fallo para auditoría. | resultado, fechaHora |
| Marcarme como leída | Usuario | Confirmo la recepción para cerrar el ciclo de comunicación. | resultado |
| Referenciar la etapa relacionada al evento | Etapa | Contextualizo el aviso con la etapa que originó la notificación. | — (relación con Etapa) |
