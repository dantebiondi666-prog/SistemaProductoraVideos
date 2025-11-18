# Matriz CLAE (CRUD) – CU07 Enviar Notificaciones Automáticas

**Proyecto:** Sistema de Gestión de Proyectos Audiovisuales  
**Caso de Uso:** CU07 – Enviar Notificaciones Automáticas (RF04, RF06)

---

## 1) Tabla CLAE

> CU07 modela el envío automático ante eventos de proyecto/etapa.  
> Se crean y actualizan entidades de notificación y registros de historial;  
> proyectos/etapas/usuarios sólo se leen.

Columnas:

- **Pry** = Proyecto  
- **Etp** = Etapa  
- **Usr** = Usuario  
- **Notif** = Notificacion (mensaje individual)  
- **SrvNotif** = ServicioNotificaciones / Pasarela / MotorNotificaciones  
- **HistNotif** = HistorialNotificaciones / registro de envíos  

| Actividad / Clase                                                 | Proyecto | Etapa | Usuario | Notificacion | ServicioNotificaciones |
|-------------------------------------------------------------------|:--------:|:-----:|:-------:|:-----------:|:----------------------:|
| Detectar evento del dominio (asignar resp., cambiar estado, etc.) |    L     |   L   |    L    |             |                        |
| Identificar destinatarios según el tipo de evento                 |    L     |   L   |    L    |             |                        |
| Obtener preferencia de canal por usuario                          |          |       |    L    |             |                        |
| Seleccionar canal preferido / canal por defecto                   |          |       |    L    |      A      |                        |
| Componer mensaje con datos (proyecto/etapa/estado)               |    L     |   L   |    L    |      C      |                        |
| Verificar disponibilidad del servicio                             |          |       |         |             |           L            |
| Intentar envío al destinatario                                    |          |       |         |      L      |           A            |
| Registrar envío exitoso                                           |          |       |         |      A      |           A            |
| Registrar intento fallido / reintentos                            |          |       |         |      A      |           A            |
| Marcar notificación para revisión (si no se logra enviar)         |          |       |         |      A      |                        |

**Leyenda:**  
**C** Crear – **L** Leer/Listar – **A** Actualizar – **E** Eliminar  

> Nota: el “historial” de envíos se modela como cambios de estado en la propia  
> entidad `Notificacion` (fecha/hora, resultado, marca de revisión),  
> sin introducir una clase `HistorialNotificaciones` separada.

---

## 2) Métodos identificados

> Los métodos se derivan de las operaciones (C/L/A/E) marcadas en la tabla.  
> Deben existir (o agregarse) en el **diagrama de clases**, **CRC** y **diagramas de secuencia**.

| Clase                | Método                                                     | Tipo | Parámetros                                      | Retorno            | Actividad asociada                                      |
|----------------------|------------------------------------------------------------|:---:|-------------------------------------------------|--------------------|---------------------------------------------------------|
| Proyecto             | `obtenerPorId(idProyecto: UUID)`                          |  L  | `idProyecto: UUID`                              | `Proyecto`         | Detectar evento / componer mensaje                      |
| Etapa                | `obtenerPorId(idEtapa: UUID)`                             |  L  | `idEtapa: UUID`                                 | `Etapa`            | Detectar evento / componer mensaje                      |
| Usuario              | `obtenerPorId(idUsuario: UUID)`                           |  L  | `idUsuario: UUID`                               | `Usuario`          | Detectar evento / identificar destinatarios             |
| Usuario              | `obtenerCanalPreferido()`                                 |  L  | —                                               | `CanalNotificacion`| Obtener preferencia de canal                            |
| Notificacion         | `crearDesdeEvento(tipo: TipoEvento, datos: DatosContexto)`|  C  | `tipo: TipoEvento`, `datos: DatosContexto`      | `Notificacion`     | Componer mensaje con datos                              |
| Notificacion         | `seleccionarCanal(canalPref: CanalNotificacion, canalDef: CanalNotificacion)` |  A | `canalPref`, `canalDef` | `void` | Seleccionar canal preferido / por defecto               |
| Notificacion         | `registrarResultado(resultado: ResultadoEnvio)`           |  A  | `resultado: ResultadoEnvio`                     | `void`             | Registrar envío exitoso / fallido / reintentos          |
| Notificacion         | `marcarParaRevision(motivo: String)`                      |  A  | `motivo: String`                                | `void`             | Marcar notificación para revisión                       |
| ServicioNotificaciones | `puedeEnviar(): boolean`                                |  L  | —                                               | `boolean`          | Verificar disponibilidad del servicio                    |
| ServicioNotificaciones | `enviar(n: Notificacion): ResultadoEnvio`               | A/L | `n: Notificacion`                               | `ResultadoEnvio`   | Intentar envío al destinatario                          |

> `DatosContexto` y `ResultadoEnvio` pueden modelarse como DTO / tipos de apoyo  
> (tipo de evento, proyecto, etapa, actor que disparó el evento, etc.).

---

## 3) Trazabilidad

| Elemento / Paso                             | Artefacto vinculado                          | Archivo / Referencia                                                                                                                            | Descripción |
|---------------------------------------------|----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|-------------|
| Detección de evento e identificación de destinatarios | Diagrama de Actividad – CU07                 | [04-actividad-enviar-notificaciones-automaticas-07.puml](../../diagramas/04-diagramas-actividades/04-actividad-enviar-notificaciones-automaticas-07.puml) | Pasos “Detectar evento del dominio” e “Identificar destinatarios”. |
| Preferencias de canal y composición de mensaje       | Diagrama de Actividad – CU07                 | [04-actividad-enviar-notificaciones-automaticas-07.puml](../../diagramas/04-diagramas-actividades/04-actividad-enviar-notificaciones-automaticas-07.puml) | “Obtener preferencia de canal”, “Seleccionar canal…”, “Componer mensaje…”. |
| Intento de envío y manejo de reintentos / revisión   | Diagrama de Actividad – CU07                 | [04-actividad-enviar-notificaciones-automaticas-07.puml](../../diagramas/04-diagramas-actividades/04-actividad-enviar-notificaciones-automaticas-07.puml) | Bloques “Intentar envío…”, “Registrar intento fallido…”, “Marcar notificación para revisión”. |
| Llamadas a `enviar(n)` y registro de resultado       | Diagrama de Secuencia – CU07                 | [05-secuencia-caso-uso-07-enviar-notificaciones-escenario-07.puml](../../diagramas/05-diagramas-secuencia/05-secuencia-caso-uso-07-enviar-notificaciones-escenario-07.puml)                                                              | Interacciones con el servicio/pasarela de notificaciones. |
| Responsabilidades de `Notificacion` y `ServicioNotificaciones` | Tarjetas CRC – Notificación / ServicioNotificaciones | [07-tarjeta-crc-servicionotificaciones.md](../../herramientas-agile/tarjetas-crc/07-tarjeta-crc-servicionotificaciones.md), [04-tarjeta-crc-notificacion.md](../../herramientas-agile/tarjetas-crc/04-tarjeta-crc-notificacion.md) | Coinciden con métodos de canal, envío y resultado. |

---

## 4) Issues e inconsistencias detectadas

| URL / referencia | Descripción de la inconsistencia / trabajo                                                                                   | Artefacto relacionado         | Acción correctiva                                                                                              | Estado    |
|------------------|-------------------------------------------------------------------------------------------------------------------------------|-------------------------------|---------------------------------------------------------------------------------------------------------------|:--------:|
| *Este archivo*   | Versión previa usaba una columna `HistNotif` (HistorialNotificaciones) no modelada en el diagrama de clases.                 | Matriz CLAE CU07              | Se elimina `HistNotif` y se modela el historial como actualizaciones sobre la propia entidad `Notificacion`. | Resuelto |

---