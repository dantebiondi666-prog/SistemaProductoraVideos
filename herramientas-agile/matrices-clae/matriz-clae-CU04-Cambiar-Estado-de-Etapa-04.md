# Matriz CLAE (CRUD) – [Cambiar Estado Etapa #04]

**Proyecto:** Sistema de Gestión de Proyectos Audiovisuales  
**Caso de Uso:** [CU4 - Cambiar Estado Etapa]  

---

## 1) Tabla CLAE

> **Estructura:**  
> - **Filas:** Actividades internas del caso de uso (acciones o pasos del flujo).  
> - **Columnas:** Clases del sistema involucradas.  
> - **Celdas:** Letras **C**, **L**, **A**, **E** según la operación que se realiza sobre la clase.  
> - Si no aplica, dejar la celda vacía.  

| Actividad / Clase                              | Proyecto | Etapa | Usuario(Coordinador) | Usuario(Responsable) | HistorialEtapa | Comentario | Adjunto | Notificacion |
|-----------------------------------------------|:--------:|:-----:|:--------------------:|:--------------------:|:--------------:|:---------:|:------:|:-----------:|
| Verificar autenticación y permisos            |          |       |          L           |          L           |                |           |        |             |
| Verificar existencia de la etapa              |          |  **L**|                      |                      |                |           |        |             |
| Mostrar estado actual y opciones              |   **L**  |  **L**|          L           |          L           |                |           |        |             |
| Actualizar estado de la etapa                 |   **L**  |  **A**|          L           |          L           |      **C**     |           |        |    **C**    |
| Registrar historial de cambio                 |   **L**  |   L   |          L           |          L           |      **C**     |           |        |             |
| Notificar interesados (Coord./Responsable)    |   **L**  |   L   |          L           |          L           |                |           |        |    **C**    |
| Confirmar cambio al actor                     |          |   L   |          L           |          L           |                |           |        |             |
| *Ext* Registrar observaciones / incidencias   |   **L**  |   L   |          L           |          L           |      **C**     |   **C**   |        |             |
| *Ext* Adjuntar link / material                |   **L**  |   L   |          L           |          L           |      **C**     |           |  **C** |             |
| *Ext* Mostrar errores de validación           |          |   L   |          L           |          L           |                |           |        |             |

> **Leyenda:**  
> **C**: Crear – **L**: Leer/Listar – **A**: Actualizar – **E**: Eliminar  

---

## 2) Métodos identificados

> Los métodos se derivan directamente de las operaciones (C/L/A/E) marcadas en la tabla.  
> Deben existir (o agregarse) en el **diagrama de clases** y reflejarse en **CRC** y **diagramas de secuencia**.

| Clase          | Método                                                                                      | Tipo | Parámetros                                             | Retorno   | Actividad asociada                          |
|----------------|---------------------------------------------------------------------------------------------|:---:|--------------------------------------------------------|-----------|---------------------------------------------|
| Usuario        | `puedeGestionarEtapas()`                                                                    |  L  | —                                                      | `boolean` | Verificar autenticación y permisos          |
| Etapa          | `obtenerPorId(idEtapa: UUID)`                                                               |  L  | `idEtapa: UUID`                                        | `Etapa`   | Verificar existencia / Mostrar estado       |
| Etapa          | `cambiarEstado(nuevoEstado: EstadoEtapa, actor: Usuario)`                                   |  A  | `nuevoEstado: EstadoEtapa`, `actor: Usuario`          | `void`    | Actualizar estado de la etapa               |
| HistorialEtapa | `registrarCambioEstado(idEtapa: UUID, estadoAnterior: EstadoEtapa, estadoNuevo: EstadoEtapa, actorId: UUID)` |  C  | `idEtapa`, `estadoAnterior`, `estadoNuevo`, `actorId` | `UUID`    | Registrar historial de cambio               |
| Comentario     | `publicarEn(etapaId: UUID, dto: NuevoComentario)`                                           |  C  | `etapaId: UUID`, `dto: NuevoComentario`               | `UUID`    | *Ext* Registrar observaciones / incidencias |
| Adjunto        | `adjuntarA(etapaId: UUID, dto: NuevoAdjunto)`                                               |  C  | `etapaId: UUID`, `dto: NuevoAdjunto`                  | `UUID`    | *Ext* Adjuntar link / material              |
| Notificacion   | `crearCambioEstado(destino: Usuario, etapa: Etapa, proyecto: Proyecto, nuevoEstado: EstadoEtapa)` |  C  | `destino`, `etapa`, `proyecto`, `nuevoEstado`         | `Notificacion` | Notificar interesados (Coord./Resp.)    |
| ServicioNotificaciones | `enviar(n: Notificacion)`                                                           | L/A | `n: Notificacion`                                     | `boolean` | Envío efectivo vía canal (mail/WhatsApp)    |

> Nota: `ServicioNotificaciones.enviar(...)` se modela en CU07 como parte del motor de notificaciones; aquí se lo referencia sólo para mantener la trazabilidad con el flujo de CU04.

---

## 3) Trazabilidad

| Elemento / Método                              | Artefacto vinculado                          | Archivo / Referencia                                                                                                                                         | Descripción |
|-----------------------------------------------|----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------|
| `Etapa.cambiarEstado(...)`                    | Diagrama de Actividad – CU04                 | [04-actividad-cambiar-estado-etapa-04.puml](../../diagramas/04-diagramas-actividades/04-actividad-cambiar-estado-etapa-04.puml)                             | Paso central “Actualizar estado de la etapa”. |
| `HistorialEtapa.registrarCambioEstado(...)`   | Diagrama de Secuencia – CU04                 | [05-secuencia-caso-uso-04-cambiar-estado-etapa-escenario-04.puml](../../diagramas/05-diagramas-secuencia/05-secuencia-caso-uso-04-cambiar-estado-etapa-escenario-04.puml)                                                                          | Mensajes de registro de historial tras el cambio. |
| `Notificacion.crearCambioEstado(...)`         | Diagrama de Actividad – CU04                 | [04-actividad-cambiar-estado-etapa-04.puml](../../diagramas/04-diagramas-actividades/04-actividad-cambiar-estado-etapa-04.puml)                             | Origen de la notificación a Coordinador/Responsable. |
| `Comentario.publicarEn(...)`, `Adjunto.adjuntarA(...)` | Diagrama de Actividad – CU04 (extensiones) | [04-actividad-cambiar-estado-etapa-04.puml](../../diagramas/04-diagramas-actividades/04-actividad-cambiar-estado-etapa-04.puml)                             | Flujos extendidos de observaciones e adjuntos. |

---

## 4) Issues e inconsistencias detectadas

| URL / referencia | Descripción de la inconsistencia                                                                                          | Artefacto relacionado          | Acción correctiva                                                                                                    | Estado    |
|------------------|----------------------------------------------------------------------------------------------------------------------------|--------------------------------|-----------------------------------------------------------------------------------------------------------------------|:---------:|
| [#134](https://github.com/dantebiondi666-prog/SistemaProductoraVideos/issues/134) | Matriz CU04 incluía clases técnicas (Servicio Notificaciones como columna) y CRUD incoherentes. | Matriz CLAE CU04               | Limitar columnas a clases de dominio (Proyecto, Etapa, Usuario, HistorialEtapa, Comentario, Adjunto, Notificacion). | Resuelto  |

