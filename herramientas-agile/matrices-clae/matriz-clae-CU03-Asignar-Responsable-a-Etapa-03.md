# Matriz CLAE (CRUD) – CU03 Asignar Responsable a Etapa

**Proyecto:** Sistema de Gestión de Proyectos Audiovisuales  
**Caso de Uso:** CU03 – Asignar Responsable a Etapa (RF02, RF04, RF06)

---

## 1) Tabla CLAE

> **Estructura:**  
> - **Filas:** Actividades internas del caso de uso (acciones o pasos del flujo).  
> - **Columnas:** Clases del sistema involucradas.  
> - **Celdas:** Letras **C**, **L**, **A**, **E** según la operación que se realiza sobre la clase.  
> - Si no aplica, dejar la celda vacía.  

| Actividad / Clase                         | Proyecto | Etapa | Usuario(Coordinador) | Usuario(RespAnterior) | Usuario(RespNuevo) | HistorialEtapa | Notificacion |
|-------------------------------------------|:-------:|:----:|:--------------------:|:---------------------:|:------------------:|:--------------:|:-----------:|
| Verificar autenticación y permisos        |    L    |  L   |          L           |                       |                    |                |             |
| Verificar existencia de la etapa          |         | **L**|                      |                       |                    |                |             |
| Identificar responsable saliente      |         |  L   |                      |         **L**         |                    |                |             |
| Actualizar asignación de responsable      |         | **A**|          L           |          L            |        **L**       |     **C**      |    **C**    |
| Registrar historial de cambios            |         |  L   |          L           |          L            |         L          |     **C**      |             |
| Notificar responsable asignado (nuevo)    |         |  L   |          L           |                       |         L          |                |    **C**    |
| Notificar responsable saliente |   |  L   |        L          |         L            |                    |                |    **C**    |
| Confirmar asignación                      |         |  L   |          L           |          L            |         L          |                |             |

> **Leyenda:**  
> **C**: Crear – **L**: Leer/Listar – **A**: Actualizar – **E**: Eliminar  


---

## 2) Métodos identificados

| Clase                  | Método                                                                 | Tipo | Parámetros                                                                 | Retorno  | Actividad asociada                     |
|------------------------|------------------------------------------------------------------------|:---:|------------------------------------------------------------------------------|----------|----------------------------------------|
| Etapa                  | `asignarResponsable(usuario: Usuario)`                                  |  A  | `usuario: Usuario`                                                          | `void`   | Actualizar asignación                  |
| Etapa                  | `listarEtapas()` / *(o)* `obtenerPorId(...)` *(si lo modelás luego)*  |  L  | — *(o `idEtapa: UUID`)*                                                     | `Etapa`  | Verificar existencia / Confirmar       |
| HistorialEtapa         | `registrarAsignacion(idEtapa: UUID, deId: UUID, aId: UUID, actorId: UUID)` |  C  | `idEtapa`, `deId`, `aId`, `actorId`                                        | `UUID`   | Registrar historial                    |
| Notificacion *(entidad)* | *(crear objeto Notificacion para asignación)*                         |  C  | `destino: Usuario`, `sobre: Etapa`, `mensaje: String`, `canal: CanalNotificacion` | `Notificacion` | Mensaje a nuevo/saliente           |
| ServicioNotificaciones | `enviar(n: Notificacion)`                                     |  L/A| `n: Notificacion`                                                           | `boolean`| Notificar nuevo y saliente (si aplica) |

---

## 3) Trazabilidad

| Elemento                      | Artefacto vinculado                         | Archivo / Referencia                                                                                                     | Descripción            |
|------------------------------|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|------------------------|
| `asignarResponsable`         | Diagrama de Actividad – CU03                | [04-actividad-asignar-responsable-etapa-03.puml](../../diagramas/04-diagramas-actividades/04-actividad-asignar-responsable-etapa-03.puml) | Paso central del flujo |
| `registrarAsignacion`        | Diagrama de Secuencia – CU03                | [05-secuencia-caso-uso-03-asignar-responsable-etapa-escenario-03.puml](../../diagramas/05-diagramas-secuencia/05-secuencia-caso-uso-03-asignar-responsable-etapa-escenario-03.puml)                                       | Evento de historial    |
| `enviarAsignacion `   | Diagrama de Actividad – CU03                | [04-actividad-asignar-responsable-etapa-03.puml](../../diagramas/04-diagramas-actividades/04-actividad-asignar-responsable-etapa-03.puml) | Notificación al nuevo  |
| `enviarReasignacion` | Diagrama de Actividad – CU03            | [04-actividad-asignar-responsable-etapa-03.puml](../../diagramas/04-diagramas-actividades/04-actividad-asignar-responsable-etapa-03.puml) | Notificación al saliente (si corresponde)|

---

## 4) Issues

| URL | Descripción | Artefacto | Acción | Estado |
|-----|-------------|-----------|--------|:-----:|
| [#130](https://github.com/dantebiondi666-prog/SistemaProductoraVideos/issues/130) | Confirmar si se notifica también al responsable saliente | Reglas de negocio / Actividad CU03 / Matriz CU03 | Se agrega columna `Usuario(RespAnterior)`, filas de identificación y de notificación al saliente, y trazabilidad en CU03. Cerrar con PR (**Fixes #130**). | Pendiente |