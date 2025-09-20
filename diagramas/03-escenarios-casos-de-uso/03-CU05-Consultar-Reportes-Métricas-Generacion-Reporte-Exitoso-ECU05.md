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
| 1. Acceder a la sección de reportes. | El actor navega a la interfaz de reportes. |
| 2. Configurar los filtros. | El sistema presenta opciones de filtro (cliente, tipo, fechas). El actor elige las que necesita. |
| 3. Aplicar los filtros. | El actor confirma para aplicar los filtros. |
| 4. Consultar y calcular datos. | El sistema ejecuta la consulta a la base de datos y realiza los cálculos necesarios. |
| 5. Mostrar visualizaciones. | El sistema presenta los resultados en tablas o gráficos. |
| 6. Exportar resultados. | El actor puede descargar el reporte en formato PDF o CSV. |

| **Condiciones, suposiciones y preguntas** | |
|---|---|
| **Precondiciones:** | Debe haber datos de proyectos y etapas en el sistema. El usuario debe tener los permisos adecuados. |
| **Poscondiciones:** | El reporte se visualiza en pantalla o se exporta a un archivo. |
| **Suposiciones:** | Se asume que la base de datos es lo suficientemente robusta para soportar las consultas. La respuesta debe ser rápida (≤3 segundos, RNF06). |
| **Reunir requerimientos:** | RF05 l sistema debe permitir consultar métricas de proyectos y etapas, RNF06 l sistema debe registrar automáticamente qué usuario completó cada tarea y los cambios de estado de cada etapa. |
| **Aspectos sobresalientes:** | La velocidad de respuesta del sistema es un requisito no funcional clave. |
| **Prioridad:** | Tiempo (Media) |
| **Riesgo:** | Tiempo - Costo (Alto) |