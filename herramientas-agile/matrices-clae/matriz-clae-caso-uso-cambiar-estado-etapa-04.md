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

| Actividad / Clase | Proyecto | Etapa | Usuario | Notificación | Comentario | Adjunto  | Servicio Notificaciones |
|--------------------|:--------:|:-----:|:-------:|:-----------------:|:-------------:|:-----:|:-----:|
| Actualizar estado de la etapa |**L**| **A** |**L** | | | | **L** |
| Registrar historial  |**L** |**C** | | | | |
| Notificar interesados | | |**L** | **C**| | |**C** |
| confirmar al actor | | **L** | **L** | **L** | |  | **L**  |
| Validad reglas de negocio |**L** |**L** |**L** | | | | |
| *Extend* Alerta retraso |**L** |**L** |**L** | **C**| | |**C** |
| *Extend* Adjuntar link a material |**L** | |**A** |**L** | | **C/A** | **L** |
| *Extend* Registrar observaciones / incidencias |**L** |**L** |**L** |**L** |**A** | |**L** |
| *Extend* Mostrar errores de validación | | |**L** | | | | |
| *Extend* Notificar al coordinador | | |**L** |**C** | | |**C** |
| *Extend* Notificar al responsable | | |**L** |**C** | | |**C** |



> **Leyenda:**  
> **C**: Crear – **L**: Leer/Listar – **A**: Actualizar – **E**: Eliminar  

---

## 2) Métodos identificados

> Los métodos se derivan directamente de las operaciones (C/L/A/E) marcadas en la tabla.  
> Cada método deberá existir en la clase correspondiente del **diagrama de clases**, y reflejarse en su **Tarjeta CRC** y en el **diagrama de secuencia** del caso de uso.

| Clase | Método | Tipo (C/L/A/E) | Parámetros (nombre: tipo) | Retorno | Actividad asociada |
|:------|--------|:--------------:|:--------------------------|:-------:|:-------------------|
| Proyecto | `obtenerEtapa(etapaId: UUID)` *(nuevo)* | **L** | `etapaID: UUID` | `ETAPA` | Busca y actualiza el estado de la etapa |
| Proyecto | `asignarResponsable(id: int, usuarioId: int)` | A | `id: int`, `usuarioId: int` | `boolean` | Asignar responsable |
| Etapa | `agregaComentario()` | **A** | - | `Void` | Agregar etapa inicial |
| Etapa | `adjuntar()` | **A** | - | `Void`|**Ext.** Adjuntar link a material|
| Etapa | `alertarRetraso(dias: int)` **(nuevo)** | **A** | `dias: int` | `Void`|**Ext.** Alerta retraso (marca/actualiza retraso)|
| Usuario | `puedeGestionarEtapas()` | **C/A** | - | `boolean` | Validar reglas de permisos |
| Notificacion | `programarEnvio()` | **C** | - | `void` | Notificar a interesados / roles |
| ServicioNotificaciones | `enviar(n: Notificacion)` | C | `n: Notificacion` | `boolean` | Notificar interesados  |

diagramas\05-diagramas-secuencia\05-secuencia-caso-uso-04-cambiar-estado-etapa-escenario-04.puml
---

## 3) Relación con otros artefactos del diseño

> Esta sección documenta la **trazabilidad** del caso de uso con los demás artefactos del modelo.  
> Cada fila establece una correspondencia entre los elementos de la matriz CLAE y los artefactos donde aparecen.

| Elemento | Artefacto vinculado | Archivo / Referencia URL | Descripción de la relación |
|----------|---------------------|--------------------------|----------------------------|
| `etapa.cambiarEstado(nuevo)` | Diagrama de Secuencia - CU04 | [`diagramas\05-diagramas-secuencia\05-secuencia-caso-uso-04-cambiar-estado-etapa-escenario-04.png`]() | Del usuario directo al objeto `etapa` |
| `Usuario.puedegestionarEtapa()` | Diagrama de Secuencia – CU04 | [`diagramas/05-diagramas-secuencia/05-secuencia-caso-uso-04-cambiar-estado-etapa-escenario-04.png`]() | Antes habiendo validado credenciales/permisos |
| `Notificacion.programarEnvio()` | Diagrama de Secuencia – CU04 | [`diagramas/04-diagramas-actividades/04-actividad-cambiar-estado-etapa-04.png`]() | Acción “Notificar interesados”: alta del registro de notificación |
| `ServicioNotificaciones.enviar(n)` | Diagrama de Secuencia – CU04 | [`diagramas/05-diagramas-secuencia/05-secuencia-caso-uso-04-cambiar-estado-etapa-escenario-04.png`]() | Envío de la notificación hacia el canal correspondiente. |
 |`Ext. Etapa.adjuntar(...)` | Diagrama de Secuencia – CU04 | [`diagramas/04-diagramas-actividades/04-actividad-cambiar-estado-etapa-04.pung`]() | Adjuntar link a material |
| `Ext. Etapa.agregarComentario()` | Diagrama de Secuencia – CU04 | [`diagramas/04-diagramas-actividades/04-actividad-cambiar-estado-etapa-04.pung`]() | Registrar observaciones / incidencias |

---

## 4) Issues e inconsistencias detectadas

> Registrar cualquier diferencia encontrada entre esta matriz y los artefactos relacionados.

| URL | Descripción de la inconsistencia | Artefacto relacionado | Acción correctiva | Estado |
|:----|:---------------------------------|:----------------------|:------------------|:------:|
| [#134](https://github.com/dantebiondi666-prog/SistemaProductoraVideos/issues/134) | Crear Matriz CLAE para Caso de Uso 04 | [`herramientas-agile\matrices-clae\matriz-clae-caso-uso-cambiar-estado-etapa-04.md`]| Completar, cerrar Issues y PR | Abierto |
| — | “Registrar historial” sin clase dedicada (Auditoría) | Modelo de dominio | (a crear) **Auditoria.registrarEvento(...)** o mantener workaround temporal en `Proyecto.registrarHistorial(...)`. | Pendiente |
| — | Firma de `ServicioNotificaciones.enviar()` sin parámetros en el boceto | Diagrama de Clases / Secuencia CU04 | Ajustar a `enviar(n: Notificacion): boolean` y reflejar en secuencia. | Pendiente |
| — | “Registrar historial” sin clase dedicada (Auditoría) | Modelo de dominio | (a crear) **Auditoria.registrarEvento(...)** o mantener workaround temporal en `Proyecto.registrarHistorial(...)`. | Pendiente |

**Estados posibles:** Abierto / Pendiente / Resuelto

---
