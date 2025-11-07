# Matriz CLAE (CRUD) – CU02 Gestionar Etapas de un Proyecto

**Proyecto:** Sistema de Gestión de Proyectos Audiovisuales  
**Caso de Uso:** CU02 – Gestionar Etapas de un Proyecto (RF02)

---

## 1) Tabla CLAE

> **Estructura:**  
> - **Filas:** Actividades internas del caso de uso (acciones o pasos del flujo).  
> - **Columnas:** Clases del sistema involucradas.  
> - **Celdas:** Letras **C**, **L**, **A**, **E** según la operación que se realiza sobre la clase.  
> - Si no aplica, dejar la celda vacía.  

| Actividad / Clase                         | Proyecto | Etapa | Usuario(Coordinador) | Usuario(Responsable) | Comentario | Adjunto | HistorialEtapa | Notificacion |
|-------------------------------------------|:-------:|:----:|:--------------------:|:--------------------:|:---------:|:------:|:--------------:|:-----------:|
| Mostrar etapas actuales                    |    L    |  L   |          L           |          L           |           |        |       L        |             |
| Agregar etapa                              |    L    | **C**|          L           |          L           |           |        |     **C**      |    **C**    |
| Editar etapa                               |    L    | **A**|          L           |          L           |           |        |     **C**      |    **C**    |
| Agregar comentario (si aplica)             |    L    |  A   |          L           |          L           |   **C**   |        |     **C**      |             |
| Adjuntar link/material (si aplica)         |    L    |  A   |          L           |          L           |           |  **C** |     **C**      |             |
| Confirmar actualización / refrescar lista  |    L    |  L   |          L           |          L           |           |        |                |             |

> **Leyenda:**  
> **C**: Crear – **L**: Leer/Listar – **A**: Actualizar – **E**: Eliminar  

---

## 2) Métodos identificados

| Clase          | Método                                                        | Tipo | Parámetros                                        | Retorno   | Actividad asociada     |
|----------------|----------------------------------------------------------------|:---:|---------------------------------------------------|-----------|------------------------|
| Etapa          | `crearEtapa(dto: NuevaEtapa)`                                  |  C  | `dto: NuevaEtapa`                                 | `UUID`    | Agregar etapa          |
| Etapa          | `actualizarEtapa(id: UUID, patch: EtapaPatch)`                 |  A  | `id: UUID`, `patch: EtapaPatch`                   | `bool`    | Editar etapa           |
| Etapa          | `listarPorProyecto(idProyecto: UUID)`                          |  L  | `idProyecto: UUID`                                | `Etapa[]` | Mostrar etapas         |
| HistorialEtapa | `registrarAlta(idEtapa: UUID, actorId: UUID)`                  |  C  | `idEtapa: UUID`, `actorId: UUID`                  | `UUID`    | Agregar etapa          |
| HistorialEtapa | `registrarEdicion(idEtapa: UUID, cambios: EtapaPatch, actorId: UUID)` |  C | `idEtapa: UUID`, `cambios: EtapaPatch`, `actorId: UUID` | `UUID` | Editar etapa |
| Comentario     | `publicarEn(etapaId: UUID, dto: NuevoComentario)`              |  C  | `etapaId: UUID`, `dto: NuevoComentario`           | `UUID`    | Agregar comentario     |
| Adjunto        | `adjuntarA(etapaId: UUID, dto: NuevoAdjunto)`                  |  C  | `etapaId: UUID`, `dto: NuevoAdjunto`              | `UUID`    | Adjuntar material      |
| Notificacion   | `enviarAltaEtapa(etapa: Etapa, interesados: Usuario[])`        |  C  | `etapa: Etapa`, `interesados: Usuario[]`          | `bool`    | Agregar etapa          |
| Notificacion   | `enviarEdicionEtapa(etapa: Etapa, interesados: Usuario[])`     |  C  | `etapa: Etapa`, `interesados: Usuario[]`          | `bool`    | Editar etapa           |

---

## 3) Trazabilidad

| Elemento                       | Artefacto vinculado                                         | Archivo / Referencia                                                  | Descripción |
|--------------------------------|--------------------------------------------------------------|------------------------------------------------------------------------|-------------|
| `crearEtapa/actualizarEtapa`   | Diagrama de Actividad – CU02                                 | `diagramas/04-diagramas-actividades/04-actividad-gestionar-etapas-proyecto-02.puml` | Flujo de alta/edición |
| `registrarAlta/registrarEdicion`| Diagrama de Secuencia – CU02                                | `diagramas/05-diagramas-secuencia/diagsecuencia02.puml`               | Mensajes de auditoría |
| `enviarAlta/EdicionEtapa`      | Diagrama de Actividad – CU02                                 | `diagramas/04-diagramas-actividades/04-actividad-gestionar-etapas-proyecto-02.puml` | Notificación al responsable |

---

## 4) Issues

| URL | Descripción | Artefacto | Acción | Estado |
|-----|-------------|-----------|--------|:-----:|
| [#129](https://github.com/dantebiondi666-prog/SistemaProductoraVideos/issues/129)   | “Eliminar etapa” no está en la actividad (sí en el UC general) | Actividad CU02 | O bien quitar del UC, o agregar subflujo y matriz `Etapa=E`, `Historial=C` | Pendiente |