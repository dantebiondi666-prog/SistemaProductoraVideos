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

| Actividad / Clase                    | Proyecto | Etapa | Usuario(Coordinador) | Usuario(RespNuevo) | HistorialEtapa | Notificacion |
|--------------------------------------|:-------:|:----:|:--------------------:|:------------------:|:--------------:|:-----------:|
| Verificar autenticación y permisos   |    L    |  L   |          L           |         L          |                |             |
| Verificar existencia de la etapa     |         | **L**|                      |                    |                |             |
| Actualizar asignación de responsable |         | **A**|          L           |         L          |     **C**      |   **C**     |
| Registrar historial de cambios       |         |  L   |          L           |         L          |     **C**      |             |
| Notificar responsable asignado       |         |  L   |          L           |         L          |                |   **C**     |
| Confirmar asignación                 |         |  L   |          L           |         L          |                |             |

> **Leyenda:**  
> **C**: Crear – **L**: Leer/Listar – **A**: Actualizar – **E**: Eliminar  


---

## 2) Métodos identificados

| Clase          | Método                                                                 | Tipo | Parámetros                                                                 | Retorno  | Actividad asociada                |
|----------------|------------------------------------------------------------------------|:---:|------------------------------------------------------------------------------|----------|-----------------------------------|
| Etapa          | `asignarResponsable(idEtapa: UUID, idUsuario: UUID)`                   |  A  | `idEtapa: UUID`, `idUsuario: UUID`                                          | `bool`   | Actualizar asignación             |
| Etapa          | `obtenerPorId(idEtapa: UUID)`                                          |  L  | `idEtapa: UUID`                                                              | `Etapa`  | Verificar existencia / Confirmar  |
| HistorialEtapa | `registrarAsignacion(idEtapa: UUID, deId: UUID, aId: UUID, actorId: UUID)` |  C  | `idEtapa: UUID`, `deId: UUID`, `aId: UUID`, `actorId: UUID`                  | `UUID`   | Registrar historial               |
| Notificacion   | `enviarAsignacion(responsable: Usuario, etapa: Etapa, proyecto: Proyecto)` |  C  | `responsable: Usuario`, `etapa: Etapa`, `proyecto: Proyecto`                 | `bool`   | Notificar asignación              |
| Validador      | `validarPermisosYUsuario(coord: Usuario, candidato: Usuario)`          |  L  | `coord: Usuario`, `candidato: Usuario`                                      | `bool`   | Verificación previa               |

---

## 3) Trazabilidad

| Elemento                      | Artefacto vinculado                                        | Archivo / Referencia                                                      | Descripción |
|------------------------------|-------------------------------------------------------------|---------------------------------------------------------------------------|-------------|
| `asignarResponsable`         | Diagrama de Actividad – CU03                                | `diagramas/04-diagramas-actividades/04-actividad-asignar-responsable-etapa-03.puml` | Paso central del flujo |
| `registrarAsignacion`        | Diagrama de Secuencia – CU03                                | `diagramas/05-diagramas-secuencia/diagsecuencia03.puml`                   | Evento de historial   |
| `enviarAsignacion`           | Diagrama de Actividad – CU03                                | `diagramas/04-diagramas-actividades/04-actividad-asignar-responsable-etapa-03.puml` | Notificación final    |

---

## 4) Issues

| URL | Descripción | Artefacto | Acción | Estado |
|-----|-------------|-----------|--------|:-----:|
| [#130](https://github.com/dantebiondi666-prog/SistemaProductoraVideos/issues/130)   | Confirmar si se notifica también al responsable saliente | Reglas de negocio | Añadir regla y, si aplica, nuevo método `enviarReasignacion(...)` | Pendiente |