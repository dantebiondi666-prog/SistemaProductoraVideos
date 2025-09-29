| **Nombre del escenario:** |Visualizacion Exitosa Tablero Control | | | |
|---|---|---|---|---|
| **Nombre del caso de uso:** | Mostrar Tablero de Control | | **ID Única:** | ECU06 |
| **Área** | Visualización / Control | | | |
| **Actor(es):** | Usuario autenticado | | | |
| **Descripción:** | Proporciona una visión general y actualizada del estado de los proyectos y sus etapas. | | | |

| **Activar Evento:** | El actor selecciona la opción "Tablero" desde el menú principal. | **Identificadores e iniciadores de caso de uso** |
|---|---|---|
| **Tipo de señal:** | ☑️ Externa | ☐ Temporal | |

| **Pasos desempeñados (ruta principal)** | **Información para los pasos** |
|---|---|
| 1. Acceder al tablero. | Vista Tablero con contexto inicial cargado: período por defecto, contadores globales, fecha/hora de última actualización. |
| 2. Cargar datos. | Consulta de proyectos y sus etapas con agregaciones de estado; uso de paginación y orden por fecha de actualización; indicador de carga mientras se procesan los datos. |
| 3. Mostrar resumen de proyectos. | Tarjetas/indicadores por proyecto con estado, avance porcentual, responsables y alertas; totales por estado (en curso, finalizados, pausados). |
| 4. Aplicar filtros. | Controles de filtro: estado, cliente, tipo y rango de fechas; al aplicar, se refresca el tablero y los contadores; manejo de “sin resultados”. |
| 5. Ver detalles. | Expansión del proyecto seleccionado para ver tabla de etapas con nombre, responsable, estado y fechas; acceso al historial de cambios. |
| 6. Navegar a vistas adicionales. | Enlaces a DetalleProyecto/{id} y opción de exportar resumen (PDF/CSV) si está habilitado; confirmación de la acción de exportación. |

| **Condiciones, suposiciones y preguntas** | |
|---|---|
| **Precondiciones:** | El usuario debe estar autenticado. Deben existir proyectos y etapas en el sistema. |
| **Poscondiciones:** | El estado general de los proyectos es visible y se facilita la navegación hacia otras secciones. |
| **Suposiciones:** | Se asume que el tablero debe mostrar la información más relevante de un vistazo. |
| **Reunir requerimientos:** | RF03 El sistema debe mostrar un panel general donde se visualice el estado de todos los proyectos y sus etapas, indicando si están en curso, finalizados o pausados y el responsable de cada etapa. |
| **Aspectos sobresalientes:** | ¿Qué filtros están disponibles por defecto y cuáles son obligatorios u opcionales? ¿Cuál es la frecuencia de actualización del tablero y si existe auto refresco? ¿Cómo se calcula el avance de cada proyecto y qué fórmula se utiliza para el porcentaje? Se permite ver todos los proyectos o se restringe por permisos del actor? ¿Qué información mínima debe mostrar cada tarjeta o indicador del proyecto? ¿Qué comportamiento se espera cuando no hay proyectos disponibles en el rango aplicado? La exportación del resumen se ofrece desde el tablero o solo desde reportes, y en qué formatos exactos?|
| **Prioridad:** | Tiempo (Alta) |
| **Riesgo:** | Tiempo - Costo (Bajo) |