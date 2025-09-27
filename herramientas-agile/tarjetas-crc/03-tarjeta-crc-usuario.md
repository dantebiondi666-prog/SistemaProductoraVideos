|  |  |  |  |
|---|---|---|---|
| Nombre de la Clase: | USUARIO | | |
| Superclase: | — | | |
| Subclases: | — | | |
| **Responsabilidades** | **Colaboradores** | **Pensamiento del objeto** | **Propiedad** |
|---|---|---|---|
| Mantener mis datos de contacto | — | Necesito que puedan identificarme y contactarme. | userId, nombre, email |
| Definir/actualizar mi rol en el sistema | — | Mi rol determina qué puedo hacer dentro del sistema. | rol |
| Exponer permisos según rol (puedeGestionarProyectos/Etapas) | — | Según mi rol, informo si tengo permiso para gestionar proyectos o etapas. | rol |
| Configurar mi canal preferido de notificación | — | Quiero recibir avisos por el medio que prefiero. | canalPreferido |
| Recibir notificaciones por mi canal preferido (delegar envío) | ServicioNotificaciones, Notificacion | Cuando ocurren eventos, recibo avisos a través del canal configurado. | canalPreferido, email |
| Marcar una notificación como leída | Notificacion | Confirmo la lectura para que quede trazabilidad de recepción. | userId |
