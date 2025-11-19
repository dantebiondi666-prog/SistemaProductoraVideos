# Matriz CLAE (CRUD) – CU01 Crear/Editar Proyecto

**Proyecto:** Sistema de Gestión de Proyectos Audiovisuales  
**Caso de Uso:** CU01 – Crear/Editar Proyecto (RF01)

---

## 1) Tabla CLAE

> Filas: actividades internas del CU01 (según actividad y secuencia).  
> Columnas: clases del dominio empleadas.  
> Celdas: **C** crear, **L** leer/listar, **A** actualizar, **E** eliminar.

| Actividad / Clase                                | Proyecto | Cliente | Usuario | Etapa | AuditoriaProyecto | Notificacion |
|--------------------------------------------------|:-------:|:------:|:------:|:----:|:-----------------:|:-----------:|
| Validar datos del proyecto                       |    L    |   L    |   L    |  L   |         L         |             |
| Persistir proyecto (crear/editar)                |  **C/A**|        |        |      |        **C**      |             |
| Registrar datos del cliente (lectura/uso)        |    L    |  **L** |        |      |                   |             |
| Definir etapas de trabajo *(preparación/lectura)*|    L    |        |        | **L**|                   |             |
| Notificar creación/edición                       |    L    |        |        |      |                   |    **C**    |
| Confirmar y redirigir a detalle                  |    L    |        |   L    |  L   |                   |             |

> **Leyenda:**  
> **C**: Crear – **L**: Leer/Listar – **A**: Actualizar – **E**: Eliminar  

---

## 2) Métodos identificados

| Clase               | Método                                                           | Tipo | Parámetros                                        | Retorno     | Actividad asociada                         |
|---------------------|------------------------------------------------------------------|:---:|---------------------------------------------------|-------------|--------------------------------------------|
| Proyecto            | `crearProyecto(dto: ProyectoDTO)`                                |  C  | `dto: ProyectoDTO`                                | `Proyecto`  | Persistir proyecto (crear)                 |
| Proyecto            | `editarProyecto(id: UUID, patch: ProyectoPatch)`                 |  A  | `id: UUID`, `patch: ProyectoPatch`                | `Proyecto`  | Persistir proyecto (editar)                |
| Proyecto            | `obtenerProyecto(id: UUID)`                                      |  L  | `id: UUID`                                        | `Proyecto`  | Validar / Confirmar / Redirigir            |
| Cliente             | `obtenerCliente(id: UUID)`                                       |  L  | `id: UUID`                                        | `Cliente`   | Registrar datos del cliente (lectura)      |
| Etapa               | `listarPorProyecto(idProyecto: UUID)`                            |  L  | `idProyecto: UUID`                                | `Etapa[]`   | Definir etapas (preparación/lectura)       |
| AuditoriaProyecto   | `logCrear(dto: ProyectoDTO, actorId: UUID)`                      |  C  | `dto: ProyectoDTO`, `actorId: UUID`               | `UUID`      | Persistir/Auditar                          |
| AuditoriaProyecto   | `logEditar(id: UUID, patch: ProyectoPatch, actorId: UUID)`       |  C  | `id: UUID`, `patch: ProyectoPatch`, `actorId: UUID`| `UUID`     | Persistir/Auditar                          |
| Notificacion        | `enviarCreacion(proyecto: Proyecto, interesados: Usuario[])`     |  C  | `proyecto: Proyecto`, `interesados: Usuario[]`    | `bool`      | Notificar creación                         |
| Notificacion        | `enviarEdicion(proyecto: Proyecto, interesados: Usuario[])`      |  C  | `proyecto: Proyecto`, `interesados: Usuario[]`    | `bool`      | Notificar edición                          |

---

## 3) Relación con otros artefactos (trazabilidad)

> Esta sección documenta la **trazabilidad** del caso de uso con los demás artefactos del modelo.  
> Cada fila establece una correspondencia entre los elementos de la matriz CLAE y los artefactos donde aparecen.

| Elemento                            | Artefacto vinculado                     | Archivo / Referencia                                                                                              | Descripción |
|-------------------------------------|-----------------------------------------|--------------------------------------------------------------------------------------------------------------------|-------------|
| `crearProyecto/editarProyecto`      | Diagrama de Actividad – CU01            | [04-actividad-crear-editar-proyecto-01.puml](../../diagramas/04-diagramas-actividades/04-actividad-crear-editar-proyecto-01.puml) | Persistir proyecto / Confirmar flujo |
| `logCrear/logEditar`                | Diagrama de Secuencia – CU01            | [05-secuencia-caso-uso-01-editar-proyecto-escenario-01.puml](../../diagramas/05-diagramas-secuencia/05-secuencia-caso-uso-01-editar-proyecto-escenario-01.puml)                                 | Mensajes posteriores al guardado (auditoría) |
| `enviarCreacion/enviarEdicion`      | Diagrama de Actividad – CU01            | [04-actividad-crear-editar-proyecto-01.puml](../../diagramas/04-diagramas-actividades/04-actividad-crear-editar-proyecto-01.puml) | Paso de notificación al final del flujo |

---

## 4) Issues e inconsistencias

> Registrar cualquier diferencia encontrada entre esta matriz y los artefactos relacionados.

| URL | Descripción | Artefacto relacionado | Acción correctiva | Estado |
|-----|-------------|-----------------------|-------------------|:-----:|
| [#127](https://github.com/dantebiondi666-prog/SistemaProductoraVideos/issues/127)   | La clase `AuditoriaProyecto` no aparece explícita en el diagrama de clases inicial | Diagrama de Clases | Añadir entidad de auditoría o documentar como servicio | Pendiente |
| [#128](https://github.com/dantebiondi666-prog/SistemaProductoraVideos/issues/128)  | “Definir etapas” en CU01 solo lee; si se decide crear etapas, actualizar matriz a `Etapa = C` y reflejar en diagramas | Actividad CU01 / Clases | Elegir variante y alinear | Pendiente |