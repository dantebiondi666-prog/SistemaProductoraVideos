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

| Actividad / Clase | Proyecto | Etapa | Usuario | Notificación | Comentario | Adjunto  | Servicio Notificaciones |
|--------------------|:--------:|:-----:|:-------:|:-----------------:|:-------------:|:-----:|:-----:|
| Verificar autenticación y permisos | | | **L** | | | | |
| Mostrar filtros disponibles | **L** | **L** | | | | | |
| Configurar y aplicar filtros | **L** | **L** | **A** | | | | |
| Modificar filtros (iteración) | **L** | **L** | **A** | | | | |
| Consultar datos según filtros | **L** | **L** | **L**  | | | |   |
| Calcular/obtener métricas | **L** | **L** |  | | | |   |
| Renderizar visualizaciones / mostrar resultados | **L** | **L** | **L** | | | |   |
| Mostrar “Sin resultados” | **L** | **L** | **L** | | | | |
| Decidir exportación (PDF/CSV) | **L** | **L** | **L** | | | | |
| Solicitar exportación | **L** | **L** | **L** | | | **C** | |
| Generar archivo y devolver enlace | **L** | **L** | | | | **C/A** | |
| Proveer enlace / Descargar archivo | | | **L** | | | **L** | |
| *Extend* Permisos insuficientes → mensaje | | | **L** | | | | |
| *Extend* Filtros inválidos → ajustar | **L** | **L** | **L/A** | | | | |


> **Leyenda:**  
> **C**: Crear – **L**: Leer/Listar – **A**: Actualizar – **E**: Eliminar  

---

## 2) Métodos identificados

> Los métodos se derivan directamente de las operaciones (C/L/A/E) marcadas en la tabla.  
> Cada método deberá existir en la clase correspondiente del **diagrama de clases**, y reflejarse en su **Tarjeta CRC** y en el **diagrama de secuencia** del caso de uso.

| Clase | Método | Tipo (C/L/A/E) | Parámetros (nombre: tipo) | Retorno | Actividad asociada |
|:------|--------|:--------------:|:--------------------------|:-------:|:-------------------|
| Proyecto | `listarPorFiltros(f: FiltrosReporte) (nuevo)` | L | `f: FiltrosReporte` | `Lista<Proyecto>` | Consultar datos |
| Proyecto | `obtenerMetricasProyecto(f: FiltrosReporte) (nuevo)` | L | `f: FiltrosReporte` | `MetricasProyecto` | Calcular/obtener métricas |    
| Etapa | `obtenerMetricasEtapas(f: FiltrosReporte) (nuevo` | L | `f: FiltrosReporte` | `MetricasEtapas` | Calcular/obtener métricas |
| Usuario | `puedeVerReportes()` | L | — | `boolean` | Verificar permisos |
| Usuario | `actualizarFiltros(filtros: FiltrosReporte) (nuevo)` | A | `filtros: FiltrosReporte` | `void` | Configurar / Modificar filtros | 
| Adjunto | `crearDesdeReporte(formato: FormatoExport, datos: MetricasProyecto) (nuevo)` | C | `formato: FormatoExport, datos: MetricasProyecto` | `Adjunto` |Generar archivo (exportación) |
| Adjunto | `descargar() (ya tenías algo similar como descargar())` | L | — | `Stream/byte[]/URL` | Descargar archivo |

---

## 3) Relación con otros artefactos del diseño

> Esta sección documenta la **trazabilidad** del caso de uso con los demás artefactos del modelo.  
> Cada fila establece una correspondencia entre los elementos de la matriz CLAE y los artefactos donde aparecen.

| Elemento | Artefacto vinculado | Archivo / Referencia URL | Descripción de la relación |
|-----------|--------------------|-----------------------|-----------------------------|
| Flujo de permisos, filtros, consulta, “sin datos”, exportar | **Diagrama de Actividad – CU05** | [`diagramas/04-diagramas-actividades/04-actividad-consultar-reportes-metricas-05.puml`]() | Origen de las filas (pasos del flujo) |
| Mensajes de consulta/cálculo/exportación | **Diagrama de Secuencia – CU05** | [`diagramas/05-diagramas-secuencia/05-secuencia-caso-uso-05-consultar-reportes-escenario-05.puml`]() | Valida que existan los métodos listados |
| Adjunto.crearDesdeReporte(...) | **Tarjeta CRC – Adjunto** | [`herramientas-agile/tarjetas-crc/crc-adjunto.md`]() |	Nueva responsabilidad: representar archivo exportado |
| Usuario.puedeVerReportes() | **Tarjeta CRC – Usuario** | [`herramientas-agile/tarjetas-crc/crc-usuario.md`]() | Autorización específica de reportes |

---

## 4) Issues e inconsistencias detectadas

> Registrar cualquier diferencia encontrada entre esta matriz y los artefactos relacionados.

| URL | Descripción de la inconsistencia | Artefacto relacionado | Acción correctiva | Estado |
|:----|:---------------------------------|:----------------------|:------------------|:------:|
| [#135](https://github.com/dantebiondi666-prog/SistemaProductoraVideos/issues/135) | Crear Matriz CLAE del Caso de Uso 05 | `herramientas-agile/matrices-clae/matriz-clae-CU05-Consultar-Reportes-y-Metricas-05.md` | Cargar secciones 1–4 y abrir issues derivados | Abierto |
| —	| Falta Usuario.puedeVerReportes() | CRC Usuario / Diagrama de clases |	Agregar método | Pendiente |
| —	| Agregar `Proyecto.listarPorFiltros(...)`, `Proyecto.obtenerMetricasProyecto(...)` | Diagrama de clases / CRC Proyecto |	Incorporar métodos de lectura | Pendiente |
| —	| Agregar `Etapa.obtenerMetricasEtapas(...)` | Diagrama de clases / CRC Etapa |	Incorporar método de lectura | Pendiente |
| —	| Agregar `Adjunto.crearDesdeReporte(formato, datos)` | Diagrama de clases / CRC Adjunto |	Incorporar método de creación desde reporte	| Pendiente |

**Estados posibles:** Abierto / Pendiente / Resuelto

---
