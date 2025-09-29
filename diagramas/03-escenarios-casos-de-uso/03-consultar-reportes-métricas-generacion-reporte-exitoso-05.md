| **Nombre del escenario:** |Generacion Reporte Exitoso | | | |
|---|---|---|---|---|
| **Nombre del caso de uso:** | Consultar Reportes/Métricas | | **ID Única:** | ECU05 |
| **Área** | Reportes y Analíticas | | | |
| **Actor(es):** | Productor / Coordinador | | | |
| **Descripción:** | Permite a los usuarios generar reportes y visualizar métricas sobre proyectos y etapas utilizando filtros específicos. | | | |

| **Activar Evento:** | El actor accede a la sección de "Reportes" en el menú. | **Identificadores e iniciadores de caso de uso** |
|---|---|---|
| **Tipo de señal:** | ☑️ Externa | ☐ Temporal | |

| **Pasos desempeñados (ruta principal)** | **Información para los pasos** |
|---|---|
| 1. Acceder a la sección de reportes. | Vista Reportes con contexto cargado: período por defecto, totales iniciales, enlaces a ayuda. |
| 2. Configurar los filtros. | Controles de filtro: cliente (selector), tipo de proyecto (publicidad/videoclip/institucional/otros), rango de fechas (desde/hasta), estado (en curso/terminado/pausado). Placeholders, validaciones básicas y botón Aplicar. |
| 3. Aplicar los filtros. | Envío de parámetros de filtrado; indicador de carga; preparación de consulta con paginación y orden. Tiempo objetivo de respuesta ≤ 3 s. |
| 4. Consultar y calcular datos. | Obtención de dataset y cálculo de métricas: proyectos finalizados por mes, duración promedio por proyecto y por etapa, tipo más frecuente, incidencias por proyecto. Manejo de “sin resultados”. |
| 5. Mostrar visualizaciones. | Render de tabla y/o gráficos; totales y promedios visibles; orden y paginación; posibilidad de ajustar filtros sin recargar la página. |
| 6. Exportar resultados. | Selector de formato PDF/CSV; generación de archivo vía Servicio de Exportación; nombre de archivo con rango de fechas y timestamp; descarga disponible. |

| **Condiciones, suposiciones y preguntas** | |
|---|---|
| **Precondiciones:** | Debe haber datos de proyectos y etapas en el sistema. El usuario debe tener los permisos adecuados. |
| **Poscondiciones:** | El reporte se visualiza en pantalla o se exporta a un archivo. |
| **Suposiciones:** | Se asume que la base de datos es lo suficientemente robusta para soportar las consultas. La respuesta debe ser rápida (≤3 segundos, RNF06). |
| **Reunir requerimientos:** | RF05 l sistema debe permitir consultar métricas de proyectos y etapas, RNF06 l sistema debe registrar automáticamente qué usuario completó cada tarea y los cambios de estado de cada etapa. |
| **Aspectos sobresalientes:** | ¿Qué filtros exactos son obligatorios y cuáles opcionales? ¿Cómo se define y mide el tiempo objetivo de respuesta ≤ 3 s y sobre qué volumen de datos? ¿Cuál es la definición precisa de cada métrica? ¿Qué comportamiento se espera cuando no hay datos para el rango seleccionado? ¿Cuál es el formato de fechas y la zona horaria usada en cálculos y exportaciones ¿Cómo se denomina el archivo exportado y qué columnas mínimas debe incluir el CSV/PDF? Se requieren permisos diferenciados para ver todos los clientes o solo los propios del actor?|
| **Prioridad:** | Tiempo (Media) |
| **Riesgo:** | Tiempo - Costo (Alto) |