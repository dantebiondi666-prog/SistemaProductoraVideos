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
| 1. Acceder al tablero. | El actor navega a la sección del tablero. |
| 2. Cargar datos. | El sistema consulta los datos relevantes y los carga. |
| 3. Mostrar resumen de proyectos. | Muestra tarjetas o elementos visuales con el estado, avance y responsables de cada proyecto. |
| 4. Aplicar filtros. | El actor puede filtrar los proyectos por estado, cliente, etc., y el sistema actualiza la vista. |
| 5. Ver detalles. | El actor puede expandir un proyecto para ver sus etapas. |
| 6. Navegar a vistas adicionales. | Se permite la navegación hacia el detalle del proyecto o la exportación del resumen. |

| **Condiciones, suposiciones y preguntas** | |
|---|---|
| **Precondiciones:** | El usuario debe estar autenticado. Deben existir proyectos y etapas en el sistema. |
| **Poscondiciones:** | El estado general de los proyectos es visible y se facilita la navegación hacia otras secciones. |
| **Suposiciones:** | Se asume que el tablero debe mostrar la información más relevante de un vistazo. |
| **Reunir requerimientos:** | RF03 El sistema debe mostrar un panel general donde se visualice el estado de todos los proyectos y sus etapas, indicando si están en curso, finalizados o pausados y el responsable de cada etapa. |
| **Aspectos sobresalientes:** | Es una vista de alto nivel que resume el estado de los proyectos. |
| **Prioridad:** | Tiempo (Alta) |
| **Riesgo:** | Tiempo - Costo (Bajo) |