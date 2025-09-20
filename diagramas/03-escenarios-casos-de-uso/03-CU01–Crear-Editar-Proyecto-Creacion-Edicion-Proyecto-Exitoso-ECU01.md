| **Nombre del escenario:** |Creación Edición Proyecto Exitoso | | | |
|---|---|---|---|---|
| **Nombre del caso de uso:** | Crear/Editar Proyecto | | **ID Única:** | ECU01 |
| **Área** | Gestión de Proyectos | | | |
| **Actor(es):** | Productor / Coordinador | | | |
| **Descripción:** | Registrar un nuevo proyecto con la información mínima requerida o actualizar un proyecto existente, asegurando la integridad de los datos. | | | |

| **Activar Evento:** | El productor selecciona “Registrar proyecto” desde el menú de Proyectos. | **Identificadores e iniciadores de caso de uso** |
|---|---|---|
| **Tipo de señal:** | ☑️ Externa | ☐ Temporal | |

| **Pasos desempeñados (ruta principal)** | **Información para los pasos** |
|---|---|
| 1. Navegar y seleccionar la opción. | El actor accede a la pantalla de proyectos y elige la acción de crear o editar. |
| 2. Cargar/mostrar el formulario | El sistema presenta un formulario con campos como nombre, cliente, fechas |
| 3. Completar los campos obligatorios. | El actor ingresa la información obligatoria y no obligatoria. |
| 4. Validar los datos ingresados. | El sistema verifica que los campos obligatorios estén completos y que no haya duplicidad en el nombre del proyecto. |
| 5. Guardar el proyecto. | Si la validación es exitosa, el sistema guarda la información en la base de datos y registra el evento en la auditoría. |
| 6. Confirmar y redirigir. | El sistema muestra un mensaje de éxito y dirige al actor a la vista de detalle del proyecto. |

| **Condiciones, suposiciones y preguntas** | |
|---|---|
| **Precondiciones:** | El usuario debe estar autenticado y tener permisos de Productor o Coordinador. El nombre del proyecto no debe existir previamente si es una creación. |
| **Poscondiciones:** | El proyecto es guardado, accesible en listados y tableros. Se ha registrado un evento de auditoría. |
| **Suposiciones:** | Se asume que el sistema de autenticación y permisos funciona correctamente. Se asume que el usuario sabe qué campos son obligatorios. |
| **Reunir requerimientos:** | RF01 - El sistema debe permitir crear y editar proyectos con los campos mínimos. |
| **Aspectos sobresalientes:** | Incluye un flujo de validación y manejo de errores. El nombre del proyecto es único. |
| **Prioridad:** | Tiempo (Alta) |
| **Riesgo:** | Tiempo - Costo (Medio) |