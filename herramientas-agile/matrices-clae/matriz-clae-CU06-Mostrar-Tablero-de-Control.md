# Matriz CLAE (CRUD) – CU06 Mostrar Tablero de Control

**Proyecto:** Sistema de Gestión de Proyectos Audiovisuales  
**Caso de Uso:** CU06 – Mostrar Tablero de Control (RF03)

---

## 1) Tabla CLAE


Columnas:

- **Pry** = Proyecto  
- **Etp** = Etapa  
- **Usr** = Usuario  
- **SrvTab** = ServicioTablero / controlador  
- **MotorTab** = MotorTablero / cálculo de métricas  
- **Cache** = CacheTablero  
- **SrvExp** = ServicioExportación  
- **ArchExp** = ArchivoExportado (PDF/CSV)

| Actividad / Clase                                         | Pry | Etp | Usr | SrvTab | MotorTab | Cache | SrvExp | ArchExp |
|-----------------------------------------------------------|:---:|:---:|:---:|:-----:|:--------:|:-----:|:------:|:------:|
| Abrir “Tablero” desde menú                                |     |     | L   | L     |          |       |        |        |
| Verificar autenticación                                   |     |     | L   | L     |          |       |        |        |
| Cargar resumen de proyectos y etapas                      | L   | L   |     | L     | L        | L/A*  |        |        |
| Mostrar tarjetas (estado, avance, responsables)           | L   | L   | L   | L     |          |       |        |        |
| Configurar y aplicar filtros                              | L   | L   |     | L     | L        | L/A   |        |        |
| Ajustar filtros y refrescar resultados (iteración)        | L   | L   |     | L     | L        | L/A   |        |        |
| Expandir proyecto y mostrar detalle de etapas             | L   | L   |     | L     |          |       |        |        |
| Solicitar exportación del resumen visible                 | L   | L   |     | L     | L        |       | C      | C      |
| Proveer enlace/descarga del archivo generado al usuario   |     |     |     | L     |          |       | L      | L      |

\* CacheTablero internamente crea/actualiza entradas cacheadas (por eso L/A).

**Leyenda:** **C** Crear – **L** Leer/Listar – **A** Actualizar – **E** Eliminar

---

## 2) Métodos identificados


| Clase             | Método                                                        | Tipo | Parámetros                                   | Retorno            | Actividad asociada                                      |
|-------------------|---------------------------------------------------------------|:---:|----------------------------------------------|--------------------|---------------------------------------------------------|
| ServicioTablero   | `cargarTablero(filtros: FiltroTablero)`                      | L   | `filtros: FiltroTablero`                     | `TableroDTO`       | Cargar resumen inicial, mostrar tarjetas                |
| ServicioTablero   | `actualizarConFiltros(filtros: FiltroTablero)`               | L   | `filtros: FiltroTablero`                     | `TableroDTO`       | Ajustar filtros y refrescar resultados                  |
| ServicioTablero   | `obtenerDetalleProyecto(idProyecto: UUID)`                   | L   | `idProyecto: UUID`                           | `DetalleProyectoDTO` | Expandir proyecto y ver etapas                        |
| MotorTablero      | `obtenerResumen(filtros: FiltroTablero)`                     | L   | `filtros: FiltroTablero`                     | `ResumenDTO`       | Consultar datos y calcular métricas                     |
| MotorTablero      | `calcularMetricas(dataset: Dataset)`                         | L   | `dataset: Dataset`                           | `MetricasDTO`      | Cálculo específico para widgets/tablas                  |
| CacheTablero      | `obtenerTablero(filtros: FiltroTablero)`                     | L   | `filtros: FiltroTablero`                     | `TableroDTO?`      | Reutilizar tablero cacheado (si existe)                 |
| CacheTablero      | `guardarTablero(filtros: FiltroTablero, t: TableroDTO)`      | C/A | `filtros`, `t`                               | `void`             | Crear/actualizar entrada de cache                      |
| ServicioExportación | `exportarTablero(t: TableroDTO, formato: FormatoExportacion)` | C | `t`, `formato`                               | `ArchivoExportado` | Generar PDF/CSV del resumen                             |
| ServicioTablero   | `obtenerEnlaceDescarga(archivo: ArchivoExportado)`           | L   | `archivo: ArchivoExportado`                  | `String`           | Proveer enlace/descarga al usuario                      |

---

## 3) Relación con otros artefactos del diseño

| Elemento                        | Artefacto vinculado                                             | Descripción de la relación                                     |
|---------------------------------|-----------------------------------------------------------------|-----------------------------------------------------------------|
| `cargarTablero()`              | 04-actividad-mostrar-tablero-control-06                         | Acciones de “cargar resumen” + “mostrar tarjetas”.             |
| `actualizarConFiltros()`       | 04-actividad-mostrar-tablero-control-06                         | Bucle de ajuste de filtros.                                    |
| `exportarTablero()`            | 04-actividad-mostrar-tablero-control-06                         | Bloque de exportación PDF/CSV.                                 |
| `cargarTablero()` / `exportarTablero()` | diagsecuencia06                                           | Mensajes entre PantallaTablero, ServicioTablero y servicios.   |
| `obtenerResumen()` / `obtenerDetalleProyecto()` | Diagrama de clases (Proyecto/Etapa)                  | Sólo lectura del dominio para armar el tablero.                |

---

## 4) Issues e inconsistencias detectadas

*(Completar si se detectan diferencias con diagramas/clases reales.)*

| URL | Descripción de la inconsistencia | Artefacto relacionado | Acción correctiva | Estado |
|----|----------------------------------|------------------------|-------------------|:------:|
|    |                                  |                        |                   |        |