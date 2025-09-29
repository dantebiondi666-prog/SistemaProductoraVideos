|  |  |  |  |
|---|---|---|---|
| Nombre de la Clase: | ETAPA | | |
| Superclase: | — | | |
| Subclases: | — | | |
| **Responsabilidades** | **Colaboradores** | **Pensamiento del objeto** | **Propiedad** |
|---|---|---|---|
| Asignar / cambiar responsable de la etapa | Usuario | Necesito un responsable activo y correcto para avanzar mi trabajo. | responsable |
| Cambiar mi estado respetando reglas de negocio | Usuario, ServicioNotificaciones / Notificacion | Actualizo mi estado (pendiente / en curso / finalizada) solo cuando corresponde y dejo trazas para avisar. | estado, fechaFinReal |
| Modificar mis fechas estimadas | — | Ajusto mi planificación para reflejar compromisos realistas. | fechaInicioEstimada, fechaFinEstimada |
| Registrar observaciones generales de la etapa | Usuario | Conservo notas relevantes asociadas a mi ejecución. | observaciones |
| Agregar comentario asociado a la etapa | Comentario, Usuario | Dejo registro detallado de aclaraciones o incidencias puntuales. | — (relación con Comentario) |
| Adjuntar material vinculado a la etapa | Adjunto, Usuario | Vinculo archivos y enlaces necesarios para trabajar. | — (relación con Adjunto) |
| Notificar cambios relevantes (responsable/estado) | ServicioNotificaciones / Notificacion | Cuando cambian responsable o estado, disparo avisos a los interesados. | estado, responsable |