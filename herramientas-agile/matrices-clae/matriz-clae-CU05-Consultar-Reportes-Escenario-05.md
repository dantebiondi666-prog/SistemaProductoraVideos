# Matriz CLAE (CRUD) – [Consultar Reportes Escenario #05]

**Proyecto:** Sistema de Gestión de Proyectos Audiovisuales  
**Caso de Uso:** [CU5 - Consultar Reportes Escenario ]  

---

## 1) Tabla CLAE

> **Estructura:**  
> - **Filas:** Actividades internas del caso de uso (acciones o pasos del flujo).  
> - **Columnas:** Clases del sistema involucradas.  
> - **Celdas:** Letras **C**, **L**, **A**, **E** según la operación que se realiza sobre la clase.  
> - Si no aplica, dejar la celda vacía.  

| Actividad / Clase                                 | Proyecto | Etapa | Usuario | Adjunto |
|---------------------------------------------------|:--------:|:-----:|:-------:|:-------:|
| Verificar autenticación y permisos                |          |       |   L     |         |
| Mostrar filtros disponibles                       |    L     |   L   |         |         |
| Configurar y aplicar filtros                      |    L     |   L   | **A**   |         |
| Modificar filtros (iteración)                     |    L     |   L   | **A**   |         |
| Consultar datos según filtros                     |    L     |   L   |   L     |         |
| Calcular / obtener métricas                       |    L     |   L   |         |         |
| Renderizar visualizaciones / mostrar resultados   |    L     |   L   |   L     |         |
| Mostrar “Sin resultados”                          |          |       |   L     |         |
| Decidir exportación (PDF/CSV)                     |    L     |   L   |   L     |         |
| Solicitar exportación                             |    L     |   L   |   L     |         |
| Generar archivo y devolver enlace                 |    L     |   L   |   L     | **C/A** |
| Proveer enlace / Descargar archivo                |          |       |   L     |   L     |
| *Ext* Permisos insuficientes → mensaje            |          |       |   L     |         |
| *Ext* Filtros inválidos → ajustar                 |    L     |   L   | **L/A** |         |

**Leyenda:**  
**C** Crear – **L** Leer/Listar – **A** Actualizar – **E** Eliminar  

> Nota: el **A** sobre Usuario en filtros representa guardar/actualizar las preferencias de filtros del usuario (opcional, pero coherente con el método `actualizarFiltros(...)`).

---

## 2) Métodos identificados

> Los métodos se derivan de las operaciones (C/L/A/E) marcadas en la tabla.  
> Deben existir (o agregarse) en el **diagrama de clases**, **CRC** y **diagramas de secuencia**.

| Clase    | Método                                           | Tipo | Parámetros                           | Retorno            | Actividad asociada                          |
|----------|--------------------------------------------------|:---:|---------------------------------------|---------------------|---------------------------------------------|
| Usuario  | `puedeVerReportes()`                            |  L  | —                                     | `boolean`           | Verificar autenticación y permisos          |
| Usuario  | `actualizarFiltros(filtros: FiltrosReporte)`    |  A  | `filtros: FiltrosReporte`             | `void`              | Configurar / Modificar filtros              |
| Proyecto | `listarPorFiltros(f: FiltrosReporte)`           |  L  | `f: FiltrosReporte`                   | `List<Proyecto>`    | Consultar datos según filtros               |
| Proyecto | `obtenerMetricasProyecto(f: FiltrosReporte)`    |  L  | `f: FiltrosReporte`                   | `MetricasProyecto`  | Calcular/obtener métricas por proyecto      |
| Etapa    | `obtenerMetricasEtapas(f: FiltrosReporte)`      |  L  | `f: FiltrosReporte`                   | `MetricasEtapas`    | Calcular/obtener métricas por etapas        |
| Adjunto  | `crearDesdeReporte(formato: FormatoExport, datos: MetricasProyecto)` |  C  | `formato`, `datos`                    | `Adjunto`           | Generar archivo (PDF/CSV)                   |
| Adjunto  | `descargar()`                                   |  L  | —                                     | `Stream/byte[]/URL` | Proveer enlace / Descargar archivo          |

> `FiltrosReporte`, `MetricasProyecto` y `MetricasEtapas` pueden modelarse como **DTO/objetos de valor** de apoyo al dominio de reportes.

---

## 3) Relación con otros artefactos (trazabilidad)

| Elemento / Paso                                      | Artefacto vinculado                      | Archivo / Referencia                                                                                                                      | Descripción |
|------------------------------------------------------|------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|-------------|
| Flujo de permisos, filtros, consulta, “sin datos”, exportar | Diagrama de Actividad – CU05             | [04-actividad-consultar-reportes-metricas-05.puml](../../diagramas/04-diagramas-actividades/04-actividad-consultar-reportes-metricas-05.puml) | Origen de las filas de la matriz CLAE.      |
| Mensajes de consulta/cálculo/exportación            | Diagrama de Secuencia – CU05             | [05-secuencia-caso-uso-05-consultar-reportes-escenario-05.puml](../../diagramas/05-diagramas-secuencia/05-secuencia-caso-uso-05-consultar-reportes-escenario-05.puml)              | Valida llamadas a `listarPorFiltros`, métricas y exportación. |
| `Adjunto.crearDesdeReporte(...)`                    | Tarjeta CRC – **Adjunto**                | [06-tarjeta-crc-adjunto.md](../../herramientas-agile/tarjetas-crc/06-tarjeta-crc-adjunto.md)                                                                    | Nueva responsabilidad: representar archivo exportado. |
| `Usuario.puedeVerReportes()` / `Usuario.actualizarFiltros(...)` | Tarjeta CRC – **Usuario**                | [03-tarjeta-crc-usuario.md](../../herramientas-agile/tarjetas-crc/03-tarjeta-crc-usuario.md)                                                                    | Autorización y preferencias de filtros de reportes. |

---

## 4) Issues e inconsistencias detectadas

| URL / referencia | Descripción de la inconsistencia / trabajo                                                         | Artefacto relacionado                            | Acción correctiva                                                                                          | Estado    |
|------------------|----------------------------------------------------------------------------------------------------|--------------------------------------------------|-----------------------------------------------------------------------------------------------------------|:--------:|
| [#135](https://github.com/dantebiondi666-prog/SistemaProductoraVideos/issues/135) | Crear Matriz CLAE del Caso de Uso 05                                     | `herramientas-agile/matrices-clae/matriz-clae-CU05-Consultar-Reportes-y-Metricas-05.md` | Se reemplaza por la matriz actual (dominio: Proyecto/Etapa/Usuario/Adjunto).                             | Resuelto |

---
