# Principio de Responsabilidad Única (SRP)

## Propósito y Tipo del Principio SOLID
SRP establece que una clase debe tener una sola razón para cambiar. Es un principio de diseño orientado a objetos que busca maximizar la cohesión y reducir el acoplamiento: al separar responsabilidades, los cambios en una regla o política impactan en una única clase y no se propagan al resto del sistema.

## Motivación
En el sistema de Vizion Estudio detectamos concentraciones de responsabilidades que generan cambios en cascada:

- En CU01 (Crear/Editar Proyecto), la clase Proyecto tiende a mezclar validación de datos, reglas de negocio y disparo de notificaciones. Si cambia la política de notificación, terminamos tocando la clase que debería enfocarse en sus propios datos/reglas.
- En CU02/CU03/CU04 (Gestionar Etapas, Asignar Responsable, Cambiar Estado), la clase Etapa administra su estado/fechas/responsable, pero además interactúa con Comentario y Adjunto. Si mezclamos “gestión documental” con “flujo de trabajo”, cualquier cambio en documentos impacta en la lógica de estado.
- En CU07 (Notificaciones Automáticas), si ServicioNotificaciones resuelve tanto el envío como el contenido del mensaje, un cambio en el formato obliga a modificar el mismo componente que orquesta los envíos.

Aplicar SRP implica que:
- Proyecto se enfoque en sus datos y reglas (nombre único, coherencia de fechas, estado), y delegue notificaciones a quienes corresponden.
- Etapa se centre en estado/fechas/responsable y colabore con Comentario/Adjunto sin absorber su lógica.
- Notificacion componga el mensaje; ServicioNotificaciones se encargue de enviarlo. Cambiar el contenido no obliga a tocar el transporte, y viceversa.

Ejemplo concreto: al editar un proyecto (CU01) se valida nombre y fechas; si la edición es correcta, se registra el cambio y se avisa a interesados. Con SRP, la validación pertenece a Proyecto; la composición del aviso a Notificacion; y el envío a ServicioNotificaciones. Un cambio futuro en el texto de aviso no requiere modificar Proyecto.

## Estructura de Clases
El diagrama refleja SRP sin introducir clases ajenas al modelo: Proyecto mantiene sus reglas y delega el aviso; Etapa se centra en su ciclo de vida y colabora con Comentario y Adjunto; Notificacion compone el mensaje y ServicioNotificaciones lo envía. El diagrama aplica SRP reubicando responsabilidades sin introducir clases nuevas: la vinculación documental pasa a Comentario/Adjunto y la programación de envíos pasa de Notificacion a ServicioNotificaciones.

![Diagrama UML – SRP](/diagramas/01-diagrama-clases/01-solid-01-srp.png)

[Ver fuente .puml](/diagramas/01-diagrama-clases/01-solid-01-srp.puml)

## Justificación Técnica
- Proyecto: una sola razón de cambio ligada a sus datos y reglas (unicidad de nombre, coherencia de fechas, estado). No contiene la lógica de “cómo” se notifica; solo desencadena la necesidad de avisar. Así, un cambio en la política de mensajes no fuerza cambios en Proyecto.
- Etapa: se ocupa de su estado y fechas y colabora con Comentario y Adjunto para registrar información contextual y material, sin absorber la lógica interna de esos objetos. Un cambio en adjuntos o comentarios no afecta la lógica de transición de estado.
- Notificacion: concentra la composición del mensaje (asunto/cuerpo) y el direccionamiento al destinatario. Si cambian plantillas o variables, se modifica aquí y no en ServicioNotificaciones ni en Proyecto/Etapa.
- ServicioNotificaciones: única responsabilidad de transportar el mensaje por el canal disponible. Si se cambia el proveedor o la política de reintentos, se modifica aquí sin afectar cómo se construye el contenido.
- - Usuario: mantiene datos de contacto, rol y canal preferido; puede disparar una notificación, pero no compone ni la transporta: la composición queda en Notificacion y el envío en ServicioNotificaciones.

Con esta distribución, cada clase tiene una razón de cambio clara:
- Proyecto/Etapa cambian si cambian sus reglas del dominio.
- Comentario/Adjunto cambian si cambian sus propias reglas/documentación.
- Notificacion cambia si cambian las plantillas o el contenido del aviso.
- ServicioNotificaciones cambia si cambian el transporte o las políticas de envío.

El resultado es un diseño más cohesivo, con menos acoplamiento y pruebas unitarias más simples (se pueden simular notificaciones sin tocar reglas de dominio y viceversa).