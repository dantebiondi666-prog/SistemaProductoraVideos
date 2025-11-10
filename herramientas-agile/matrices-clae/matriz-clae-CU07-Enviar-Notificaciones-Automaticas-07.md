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

| Actividad / Clase                                                 | Pry | Etp | Usr | Notif | SrvNotif | HistNotif |
|-------------------------------------------------------------------|:---:|:---:|:---:|:----:|:-------:|:--------:|
| Detectar acción relevante (asignar resp., cambiar estado, etc.)   | L   | L   | L   |      | L       |          |
| Identificar destinatarios según tipo de evento                    | L   | L   | L   |      | L       |          |
| Obtener preferencia de canal por usuario                          |     |     | L   |      | L       |          |
| Seleccionar canal preferido / canal por defecto                   |     |     | L   |      | L/A     |          |
| Componer mensaje con datos de proyecto/etapa                      | L   | L   | L   | C    |         |          |
| Intentar envío por canal seleccionado                             |     |     |     | L    | L/A     |          |
| Registrar envío exitoso en historial                              |     |     |     | A    | A       | C        |
| Registrar intento fallido / reintentos                            |     |     |     | A    | A       | C        |
| Marcar notificación para revisión (si no se logra enviar)         |     |     |     | A    | A       | A        |

**Leyenda:** **C** Crear – **L** Leer/Listar – **A** Actualizar – **E** Eliminar

---

## 2) Métodos identificados

| Clase                  | Método                                                               | Tipo | Parámetros                                                 | Retorno            | Actividad asociada                                  |
|------------------------|----------------------------------------------------------------------|:---:|------------------------------------------------------------|--------------------|-----------------------------------------------------|
| MotorNotificaciones    | `procesarEvento(tipo: TipoEvento, entidadId: UUID)`                 | L   | `tipo`, `entidadId`                                       | `void`             | Punto de entrada: detectar acción relevante         |
| MotorNotificaciones    | `identificarDestinatarios(tipo: TipoEvento, entidadId: UUID)`       | L   | `tipo`, `entidadId`                                       | `List<Usuario>`    | Resolver quién debe ser notificado                  |
| Usuario                | `obtenerCanalPreferido(): CanalNotificacion`                        | L   | —                                                          | `CanalNotificacion`| Usado para elegir canal por usuario                 |
| Notificacion           | `crearDesdeEvento(tipo: TipoEvento, datos: DatosContexto)`          | C   | `tipo`, `datos`                                           | `Notificacion`     | Componer mensaje con datos de proyecto/etapa       |
| ServicioNotificaciones | `puedeEnviar(): boolean`                                            | L   | —                                                          | `boolean`          | Evaluar límite diario / disponibilidad              |
| ServicioNotificaciones | `enviar(n: Notificacion): ResultadoEnvio`                           | L/A | `n: Notificacion`                                         | `ResultadoEnvio`   | Intentar envío por canal seleccionado               |
| HistorialNotificaciones| `registrarEnvio(n: Notificacion, r: ResultadoEnvio)`                | C   | `n`, `r`                                                  | `void`             | Guardar envío exitoso o fallido                     |
| HistorialNotificaciones| `marcarParaRevision(n: Notificacion, motivo: String)`               | A   | `n`, `motivo`                                             | `void`             | Dejar pendiente cuando no se logra entregar         |

> Nota: `ServicioNotificaciones` y/o `PasarelaNotificaciones` implementan la lógica de canal (email/WhatsApp) apoyados en OCP/DIP; `MotorNotificaciones` es quien orquesta según CU03/CU04.

---

## 3) Relación con otros artefactos del diseño

| Elemento                         | Artefacto vinculado                                  | Descripción de la relación                             |
|----------------------------------|------------------------------------------------------|-------------------------------------------------------|
| `procesarEvento()`              | 04-actividad-enviar-notificaciones-automaticas-07    | Representa “Detectar evento del dominio”.             |
| `identificarDestinatarios()`    | 04-actividad-enviar-notificaciones-automaticas-07    | Paso “Identificar destinatarios según el evento”.     |
| `crearDesdeEvento()`            | 04-actividad-enviar-notificaciones-automaticas-07    | Paso “Componer mensaje con datos”.                    |
| `enviar(n)`                     | diagsecuencia07; diagramas OCP/DIP de notificaciones | Uso del servicio / pasarela desacoplado por interfaz. |
| `registrarEnvio()` / `marcarParaRevision()` | 04-actividad-enviar-notificaciones-automaticas-07 | Pasos de registro de historial y manejo de fallos.    |

---

## 4) Issues e inconsistencias detectadas

*(Completar cuando se contraste con el puml final y CRCs.)*

| URL | Descripción de la inconsistencia | Artefacto relacionado | Acción correctiva | Estado |
|----|----------------------------------|------------------------|-------------------|:------:|
|    |                                  |                        |                   |        |