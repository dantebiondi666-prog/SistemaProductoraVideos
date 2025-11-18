# Sesión JAD – Sistema de Gestión de Proyectos Audiovisuales

**Fecha:** 14/03/2025 – 11:00hs  
**Lugar:** `02 - Transcripción Gemini GMeet - Reunión interna – Área de Producción & Desarrollo` – Fecha: 14/03/2025.pdf -  `NotebookLM`  

**Participantes:**  
- Laura González (Productora general – Vizion Estudio)  
- Martín Suárez (Responsable de Edición – Vizion Estudio)  
- Carla Paredes (Asistente de producción – Vizion Estudio)  
- Marcos Díaz (Analista funcional – SolucionesDev)  
- Julieta Romero (UX/UI – SolucionesDev)  

---

## 1) Objetivo de la sesión
Definir responsabilidades, reglas funcionales y restricciones del sistema orientado a objetos, para validar el modelo de clases y el flujo de trabajo representado en los diagramas actuales (CU01 – Crear/Editar Proyecto, CU02 – Gestionar Etapas).

---

## 2) Matriz de Registro JAD  

Mínimo 10 registros completos extraídos de la Sesión JAD.

| **Pregunta Clave (según guía JAD)** | **Respuesta / Decisión del Usuario** | **Clases Candidatas** | **Atributos / Métodos / Responsabilidades Detectadas** | **Observaciones** |
|-------------------------------------|--------------------------------------|:---------------------:|--------------------------------------------------------|-------------------|
| ¿Quién crea/modifica etapas en un proyecto? | Productor/Admin. | `Usuario`, `Proyecto`, `Etapa`, `HistorialEtapa` | `Usuario.rol`, `Usuario.puedeGestionarEtapas()`, `Proyecto.agregarEtapa()`, `Proyecto.eliminarEtapa()`, `Etapa.crearEtapa(...)`, `Etapa.actualizarEtapa(...)`, `Etapa.eliminarEtapa(...)`, `HistorialEtapa.registrarAlta/registrarEdicion/registrarBaja(...)` | Reglas de permiso por rol; se refleja en CU02, CU03, CU04 y en CRC Usuario. |
| ¿El flujo de etapas es fijo o configurable? | Configurable según tipo de trabajo. | `Proyecto`, `Etapa`, *(posible)* `PlantillaEtapas` | `Proyecto.tipo`, `Proyecto.etapas[]` (orden configurable), `Etapa.orden`. | Para la **v1** se configuran etapas “a mano” por proyecto (CU01/CU02). La idea de `PlantillaEtapas` queda registrada como mejora futura (no modelada aún en el diagrama de clases). |
| ¿Puede haber más de un responsable por etapa? | Responsable principal + ayudantes/subtareas. | `Etapa`, `Usuario`, *(posible)* `Tarea` | `Etapa.responsable` (responsable principal en v1), *(futuro)* `Etapa.ayudantes[]`, `Tarea.asignados[]`. En CU03: `HistorialEtapa.registrarAsignacion(...)`, notificaciones a responsable nuevo/saliente. | En la **v1** se implementa un único responsable por etapa (CU03). La noción de ayudantes/subtareas queda como extensión futura. |
| ¿Se registran observaciones internas? | Sí, por etapa. | `Comentario`, `Etapa`, `Usuario`, `HistorialEtapa` | `Comentario.texto`, `Comentario.autor`, `Comentario.fecha`, `Comentario.tipo` (comentario/incidencia), `Comentario.publicarEn(etapaId, dto)`, `Etapa.agregarComentario()`, `HistorialEtapa.registrarCambioEstado(...)` (cuando la observación afecta estado). | Diferenciar comentario informativo vs. incidencia. Se usa en flujos extendidos de CU02 y CU04. |
| ¿Se necesita historial de incidencias/retrasos? | Sí, con fecha, usuario, tipo. | `Proyecto`, `Etapa`, `AuditoriaProyecto`, `HistorialEtapa` | `AuditoriaProyecto.logCrear(...)`, `AuditoriaProyecto.logEditar(...)` para CU01 (proyectos); `HistorialEtapa.registrarAlta/registrarEdicion/registrarBaja/registrarCambioEstado/registrarAsignacion(...)` para CU02–CU04. | Se decidió **separar**: `AuditoriaProyecto` para cambios de proyecto y `HistorialEtapa` para cambios de etapa. Ambos aparecen en matrices CLAE y deben reflejarse en diagrama de clases y CRC. |
| ¿Se guardan enlaces a material externo (Drive/Vimeo)? | Sí, guardar links; archivos nativos en versión futura. | `Adjunto`, `Etapa`, `Proyecto` | `Adjunto.url`, `Adjunto.tipo` (LINK/ARCHIVO), `Adjunto.fechaAdjunto`, `Etapa.adjuntar(adjunto)`, `Adjunto.generarPreview()`, `Adjunto.crearDesdeReporte(...)` (para exportes de reportes). | Para v1: sólo URL externas y archivos exportados (PDF/CSV) de reportes; se deja preparado el tipo para archivos físicos a futuro. |
| ¿Se envían notificaciones automáticas? | Sí: al completar etapa y en cambios de estado relevantes de proyecto/etapa. | `Notificacion`, `ServicioNotificaciones`, `Usuario`, `Etapa`, `Proyecto` | `Notificacion.destino`, `Notificacion.mensaje`, `Notificacion.canal`, `Usuario.obtenerCanalPreferido()`, `Notificacion.crearDesdeEvento(...)`, `Notificacion.crearCambioEstado(...)`, `ServicioNotificaciones.puedeEnviar()`, `ServicioNotificaciones.enviar(n)`, `Notificacion.registrarResultado(...)`, `Notificacion.marcarParaRevision(...)`. | Eventos disparadores en CU03 y CU04; la orquestación y envío se agrupan en CU07 (**Enviar Notificaciones Automáticas**). |
| ¿Fechas límite por etapa? | Sí, obligatorias. | `Etapa` | `Etapa.fechaInicioEstimada`, `Etapa.fechaFinEstimada` *(fecha límite)*, `Etapa.fechaFinReal`. | Sirven para detectar retrasos, calcular métricas (CU05) y alimentar alertas/notificaciones (CU07). |
| ¿Cuánta anticipación para alertas de retraso? | 24 h antes de la fecha límite. | `Etapa`, `Notificacion`, *(servicio de scheduling / motor de notificaciones)* | Regla de negocio: “alertar 24h antes de `fechaFinEstimada` si la etapa sigue pendiente/en proceso”. Se implementa con `Notificacion.crearDesdeEvento(...)` + `ServicioNotificaciones.enviar(...)` disparados por un evento de “etapa próxima a vencer”. | Se deja parametrizable el umbral (24h por defecto). La lógica de programación de eventos se ubica en CU07, no como método directo de `Etapa`. |
| ¿Prioridades de tareas/etapas? | Alta/Media/Baja; colores en tablero. | `Etapa`, *(posible)* `Tarea`, *(vista Tablero)* | `Etapa.prioridad : PrioridadEtapa`, mapping visual (rojo/amarillo/verde) en Tablero/Reportes. | Consistencia entre dominio (`enum PrioridadEtapa`) y UI (colores). Prioridad se usa en CU05/CU06 como criterio de filtro/orden. |
| ¿Vista principal requerida? | Tablero con proyectos activos y filtros (estado, responsable, tipo). | `Proyecto`, `Usuario`, *(vista)* Tablero | `Proyecto.estado`, `Proyecto.tipo`, filtros por responsable/cliente/fechas; consultas `Proyecto.listarPorFiltros(...)`, `Etapa.listarPorProyecto(...)`. | Se implementa como CU06 (**Mostrar Tablero de Control**). El “Tablero” no se modela como clase de dominio; se representa como servicio/vista que consume las consultas de Proyecto/Etapa/Usuario. |
| ¿Métricas: tiempo estimado vs real? | Sí, por etapa y proyecto; estadísticas por tipo. | `Proyecto`, `Etapa`, DTOs de métricas | `Proyecto.obtenerMetricasProyecto(filtros: FiltrosReporte) : MetricasProyecto`, `Etapa.obtenerMetricasEtapas(filtros: FiltrosReporte) : MetricasEtapas`, estructuras `FiltrosReporte`, `MetricasProyecto`, `MetricasEtapas`. | Base de CU05 (**Consultar Reportes y Métricas**). Los DTO de métricas se usan también para la vista de Tablero (CU06) y exportación. |
| ¿Versionado/etiquetado de entregas? | Sí, para múltiples versiones de video. | `Adjunto`, *(posible)* `Entrega` | *(futuro)* `Adjunto.version`, `Entrega.tag`, `Entrega.fecha`. | Requisito identificado pero **fuera de alcance de la v1**. Se documenta para una posible evolución (no modelado en el diagrama de clases final). |
| ¿Plataforma y UX? | Web responsive; simplicidad; equipo no técnico. | *(no dominio)* | Reglas de usabilidad y perfiles de acceso; mensajes claros “sin resultados”, manejo simple de filtros. | Impacta en validaciones y feedback (por ejemplo en CU05/CU06: mostrar “Sin resultados”, mensajes de permisos insuficientes, etc.). |

---

## 3) Issues e inconsistencias detectadas

| URL | Descripción de la inconsistencia / trabajo | Artefacto relacionado | Acción correctiva concreta | Estado |
|-----|--------------------------------------------|-----------------------|----------------------------|:-----:|
| #137 | **Análisis JAD** – cargar documento y trazabilidad | `herramientas-agile/analisis-jad/analisis-jad.md` | Actualizar el análisis JAD (este archivo), vincular explícitamente cada decisión con matrices CLAE, CRC y diagramas (CU01–CU07). | En revisión |
| #136 | **Detectar inconsistencias desde CLAE** (cruce CLAE ↔ CRC/Diagramas) | Matrices CLAE, CRC, Diagramas | Varias inconsistencias ya fueron detectadas y corregidas (AuditoriaProyecto, HistorialEtapa, CU05, CU06, CU07). Mantener el issue abierto para futuras divergencias entre modelos. | Abierto |
| #135 | **Crear Matriz CLAE del Caso de Uso 05** | `herramientas-agile/matrices-clae/matriz-clae-CU05-Consultar-Reportes-y-Metricas-05.md` | Se creó la matriz completa de CU05, se definieron `FiltrosReporte`, `MetricasProyecto`, `MetricasEtapas` y los métodos de lectura/exportación. | Resuelto |
| — | Permiso explícito: **solo Productor/Admin** crea/modifica etapas | CRC Usuario / Reglas CU02–CU04 | Se incorporó `Usuario.puedeGestionarEtapas()` y se refleja en CU02–CU04. Falta verificar que aparezca en todas las CRC y secuencias correspondientes. | Pendiente (dentro de #136) |
| — | Flujo **configurable** de etapas no modelado | Diagrama de Clases / Especificación | Mantener el diseño actual (configuración manual por proyecto) y documentar la posible `PlantillaEtapas` como mejora futura. | Pendiente (dentro de #136) |
| — | **Responsable principal + ayudantes** por etapa | Clase Etapa / CRC | En la v1 sólo se implementa un responsable (`Etapa.responsable`). Registrar en la especificación la idea de ayudantes/subtareas para futuras iteraciones. | Pendiente (dentro de #136) |
| — | **Observaciones** tipadas (incidencia/comentario) | Comentario / CRC | Definir `Comentario.tipo` (por ejemplo, COMENTARIO / INCIDENCIA) y reflejarlo en CRC y en CU04 (extensión de observaciones). | Pendiente (dentro de #136) |
| — | **Historial** de cambios/incidencias | Proyecto / Auditoría / HistorialEtapa | Se incorporaron `AuditoriaProyecto` (para CU01) y `HistorialEtapa` (para CU02–CU04). Revisar diagramas para garantizar que los logs se canalicen a estas clases. | En curso (parte de #136) |
| — | **Links externos** vs archivos | Adjunto | Se definió `Adjunto.tipo` y métodos `Adjunto.adjuntarA(...)`, `Adjunto.crearDesdeReporte(...)`. Verificar consistencia en CRC Adjunto y matrices CU02/CU04/CU05. | Resuelto (ver matrices 2, 4 y 5) |
| — | **Alerta 24h** antes de fecha límite | Etapa / CU07 | La regla de negocio se cubre mediante eventos de “etapa próxima a vencer” + CU07 (notificaciones automáticas). Falta detallar en la especificación técnica cómo se disparan estos eventos. | Pendiente (dentro de #136) |
| — | **Prioridad** (alta/media/baja) | Etapa / (Tarea) | Modelar `Etapa.prioridad` como enum y usarla en filtros/orden de reportes/cuadro de mando. | Pendiente (dentro de #136) |
| — | **Métricas** (estimado vs real) sin DTOs/métodos | Proyecto/Etapa/CU05 | Se agregaron `FiltrosReporte`, `MetricasProyecto`, `MetricasEtapas` y métodos de lectura en CU05. | Resuelto (ver Matriz CU05 y CRC) |
| — | **Exportación** (PDF/CSV) como Adjunto | Adjunto / CU05 | Se modeló `Adjunto.crearDesdeReporte(formato, datos)` y `Adjunto.descargar()` en CU05. Revisar implementación en diagramas de secuencia. | Resuelto (ver Matriz CU05) |