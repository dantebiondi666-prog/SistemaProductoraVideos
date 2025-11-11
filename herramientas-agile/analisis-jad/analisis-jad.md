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
Minimo 10 registros completos extraidos de la Sesión JAD.

| **Pregunta Clave (según guía JAD)** | **Respuesta / Decisión del Usuario** | **Clases Candidatas** | **Atributos / Métodos / Responsabilidades Detectadas** | **Observaciones** |
|------------------------------------|--------------------------------------|:------------------:|--------------------------------------------------------|------------------|
| ¿Quién crea/modifica etapas en un proyecto? | Productor/Admin. | `Usuario` `Proyecto` `Etapa` | `Usuario.rol`, `Proyecto.agregarEtapa()` `Proyecto.quitarEtapa()` `Etapa.asignarResponsable()` | Reglas de permiso por rol; reflejar en CRC Usuario |
| ¿El flujo de etapas es fijo o configurable? | Configurable según tipo de trabajo | Proyecto, Etapa | `Proyecto.tipo` `Proyecto.etapas[]` (orden configurable) `Etapa.orden` | Implica modelar plantillas o clonación de flujos a futuro |
| ¿Puede haber más de un responsable por etapa? | Responsable principal + ayudantes/subtareas |	Etapa, Usuario, (posible) Tarea | `Etapa.responsablePrincipal`, `Etapa.ayudantes[]`, `Tarea.asignados[]` |	Subtareas opcionales (versión posterior). Mantener la noción de “principal” |
| ¿Se registran observaciones internas?	| Sí, por etapa | Comentario, Etapa, Usuario | `Comentario.texto`, `Comentario.autor`, `Comentario.fecha`, `Etapa.agregarComentario()` | Diferenciar comentario vs. incidencia (tipo) |
| ¿Se necesita historial de incidencias/retrasos? | Sí, con fecha, usuario, tipo | Proyecto, Etapa, (posible) Auditoria | `Proyecto.registrarHistorial(...)` o `Auditoria.registrarEvento(...)`	| Decidir si crear clase Auditoria dedicada |
| ¿Se guardan enlaces a material externo (Drive/Vimeo)?	| Sí, guardar links; archivos nativos en versión futura | Adjunto (como Link), Etapa, Proyecto	| `Adjunto.url`, `Adjunto.tipo`, `Etapa.adjuntar()`, `Adjunto.generarPreview()` | Para v1: solo URL. Dejar campo y tipo preparado|
| ¿Se envían notificaciones automáticas? | Sí: al completar etapa y en cambios de estado relevantes de proyecto | Notificación, Servicio Notificaciones, Usuario, Etapa, Proyecto |	`Notificación.destino`, `Notificación.mensaje`, `ServicioNotificaciones.enviar(n)` | Definir eventos disparadores y destinatarios por rol |
| ¿Fechas límite por etapa? | Sí, obligatorias | Etapa	| `Etapa.fechaLimite` |	Precondición para alertas y cálculo de retraso |
| ¿Cuánta anticipación para alertas de retraso? | 24 h antes de la fecha límite | Etapa, Notificación |	`Etapa.alertarRetraso(24h)`, regla programada	| Parametrizar umbral (24h configurable) |
| ¿Prioridades de tareas/etapas? |	Alta/Media/Baja; colores en tablero | Etapa, (posible) Tarea, Tablero |	`Etapa.prioridad`, mapping visual (rojo/amarillo/verde) | Consistencia entre dominio (enum) y UI |
| ¿Vista principal requerida? | Tablero con proyectos activos y filtros (estado, responsable, tipo) | Proyecto, Usuario, Tablero | `Proyecto.estado`, `Proyecto.tipo`, filtros por responsable | No modelar UI como clase si no tiene lógica; usar Tablero solo si se decide como servicio/vista lógica |
| ¿Métricas: tiempo estimado vs real? |	Sí, por etapa y proyecto; estadísticas por tipo | Proyecto, Etapa, (DTOs) Métricas | `Etapa.fechaInicio/FinReal`, `Proyecto.obtenerMetricasProyecto(filtros)` | Base para CU05 (reportes y exportación) |
| ¿Versionado/etiquetado de entregas? |	Sí, para múltiples versiones de video | Adjunto, (posible) Entrega | `Adjunto.version`, `Entrega.tag`, `Entrega.fecha` | Útil para trazabilidad; puede convivir con links externos |
| ¿Plataforma y UX? | Web responsive; simplicidad; equipo no técnico | (no dominio)	| Reglas de usabilidad y perfiles de acceso	| Impacta validaciones y mensajes claros (p. ej., “sin resultados”) |

---

## 3) Issues e inconsistencias detectadas

| URL | Descripción de la inconsistencia / trabajo | Artefacto relacionado | Acción correctiva concreta | Estado |
|-----|--------------------------------------------|-----------------------|----------------------------|:-----:|
| #137 | **Análisis JAD** – cargar documento y trazabilidad | herramientas-agile/analisis-jad/analisis-jad.md | Completar secciones finales y relacionar con matrices/CRC | Abierto |
| #136 | **Detectar inconsistencias desde CLAE** (cruce CLAE ↔ CRC/Diagramas) | Matrices CLAE, CRC, Diagramas | Listar diferencias (métodos, permisos, DTOs, reglas no modeladas) y abrir sub-issues | Abierto |
| #135 | **Crear Matriz CLAE del Caso de Uso 05** | herramientas-agile/matrices-clae/matriz-clae-CU05-Consultar-Reportes-y-Metricas-05.md | Completar tabla (columnas estándar), métodos identificados y trazabilidad a CU05 | Abierto |
| — | Permiso explícito: **solo Productor/Admin** crea/modifica etapas | CRC Usuario / Reglas CU02–CU04 | Agregar Usuario.puedeGestionarEtapas() y documentar en CRC + secuencias | Pendiente (reportar en #136) |
| — | Flujo **configurable** de etapas no modelado | Diagrama de Clases / Especificación | Evaluar PlantillaEtapas o clonación por tipo; registrar decisión | Pendiente (reportar en #136) |
| — | **Responsable principal + ayudantes** por etapa | Clase Etapa / CRC | Atributos responsablePrincipal, ayudantes[]; ajustar métodos | Pendiente (reportar en #136) |
| — | **Observaciones** tipadas (incidencia/comentario) | Comentario / CRC | Campo Comentario.tipo y validaciones en CU04 | Pendiente (reportar en #136) |
| — | **Historial** de cambios/incidencias | Proyecto / Auditoría | Proyecto.registrarHistorial(...) o clase Auditoria | Pendiente (reportar en #136) |
| — | **Links externos** vs archivos | Adjunto | Atributo tipo (LINK/ARCHIVO) + crearDesdeLink(url) | Pendiente (reportar en #136) |
| — | **Alerta 24h** antes de fecha límite | Etapa / Notificación | Etapa.alertarRetraso(anticipacionHoras=24) | Pendiente (reportar en #136) |
| — | **Prioridad** (alta/media/baja) | Etapa / (Tarea) | Enum Prioridad + Etapa.prioridad | Pendiente (reportar en #136) |
| — | **Métricas** (estimado vs real) sin DTOs/métodos | Proyecto/Etapa/CU05 | FiltrosReporte, MetricasProyecto/Etapas + métodos | Pendiente (reportar en #136) |
| — | **Exportación** (PDF/CSV) como Adjunto | Adjunto / CU05 | Adjunto.crearDesdeReporte(formato, datos) + descargar() | Pendiente (reportar en #136) |