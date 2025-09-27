|  |  |  |  |
|---|---|---|---|
| Nombre de la Clase: | SERVICIONOTIFICACIONES | | |
| Superclase: | — | | |
| Subclases: | — | | |
| **Responsabilidades** | **Colaboradores** | **Pensamiento del objeto** | **Propiedad** |
|---|---|---|---|
| Configurar el proveedor de envío | — | Necesito saber con qué proveedor operar para transportar mensajes. | proveedor |
| Verificar si puedo enviar según el límite diario | — | Controlo mi cupo antes de procesar nuevos mensajes. | limiteDiario, tiempoUltimoEnvio |
| Enviar una notificación por el canal solicitado | Notificacion | Efectúo el transporte del mensaje al destino por el canal indicado. | tiempoUltimoEnvio |
| Registrar el instante del último envío realizado | — | Actualizo mi marca temporal para respetar límites y monitoreo. | tiempoUltimoEnvio |
| Informar el resultado del envío al emisor | Notificacion | Devuelvo el estado del intento para que quede trazabilidad. | — |