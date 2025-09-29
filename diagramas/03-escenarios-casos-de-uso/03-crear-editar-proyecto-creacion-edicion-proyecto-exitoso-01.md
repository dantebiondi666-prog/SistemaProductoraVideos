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
| 1. Navegar y seleccionar la opción. | Navegación: Menú Proyectos → acción Nuevo o Editar (comandos/links disponibles en la vista de lista). |
| 2. Cargar/mostrar el formulario | Formulario `ProyectoForm` con campos: nombre, cliente, fechas inicio/fin estimadas, estado, observaciones. Controles: Guardar / Cancelar. |
| 3. Completar los campos obligatorios. | Datos ingresados en `ProyectoForm`: indicadores de obligatoriedad (asterisco), placeholders, máscaras de fecha y selector de cliente. |
| 4. Validar los datos ingresados. | Validaciones de servidor/cliente: obligatorios completos; formato de fecha; unicidad de nombre (normalizar mayúsculas/espacios); coherencia inicio ≤ fin. |
| 5. Guardar el proyecto. | Persistencia y auditoría: registro Proyecto{ id, nombre, cliente, fechas, estado, observaciones }; ProjectAuditLog{ userId, acción (crear/editar), timestamp }. |
| 6. Confirmar y redirigir. | Confirmación y redirección: mensaje de éxito (toast/snackbar) y navegación a DetalleProyecto/{id}; listados/tablero quedan listos para refrescar. |

| **Condiciones, suposiciones y preguntas** | |
|---|---|
| **Precondiciones:** | El usuario debe estar autenticado y tener permisos de Productor o Coordinador. El nombre del proyecto no debe existir previamente si es una creación. |
| **Poscondiciones:** | El proyecto es guardado, accesible en listados y tableros. Se ha registrado un evento de auditoría. |
| **Suposiciones:** | Se asume que el sistema de autenticación y permisos funciona correctamente. Se asume que el usuario sabe qué campos son obligatorios. |
| **Reunir requerimientos:** | RF01 - El sistema debe permitir crear y editar proyectos con los campos mínimos. |
| **Aspectos sobresalientes:** | ¿Estado inicial por defecto al crear? Regla exacta de unicidad de nombre: ¿ignorar mayúsculas, tildes y espacios? ¿Qué campos son obligatorios exactamente? Formato de fecha (DD/MM/AAAA) y zona horaria usada en auditoría. ¿Se requiere notificar a cliente/administrador al crear/editar? |
| **Prioridad:** | Tiempo (Alta) |
| **Riesgo:** | Tiempo - Costo (Medio) |