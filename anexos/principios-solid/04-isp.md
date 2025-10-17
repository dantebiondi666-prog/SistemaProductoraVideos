# Principio de Segregación de Interfaces (ISP)

## Propósito
El Principio de Segregación de Interfaces (ISP) establece que **ninguna clase debe estar forzada a implementar métodos que no utiliza**.  
En otras palabras, es preferible **tener varias interfaces pequeñas y específicas** en lugar de una única interfaz "gorda" con demasiadas responsabilidades.

## Motivación
En *SistemaProductoraVideos* detectamos una **interfaz gorda** en servicios que combinaban consultas y operaciones que no todos los clientes usan. Por ejemplo, un único servicio de reportes/tablero con métodos para **consultar métricas** (RF05, CU05/CU06) y también **exportar** (PDF/CSV). 
- **Problema:** el **Tablero** (CU06) solo necesita consultar y filtrar; no debería depender de métodos de exportación.  
- **Otro caso:** servicios de **Etapas** mezclaban lectura y escritura; **Reportes** (RF05) solo requiere **lectura**, mientras que **Gestión de Etapas** (RF02/CU02–CU04) usa **escritura** y cambios de estado.

> Esto viola ISP: *los clientes no deben depender de métodos que no utilizan*.

## Aplicación de ISP en el proyecto
Separamos interfaces grandes en **interfaces específicas por rol de uso**:

- `IReportesConsulta` → consultas/agregaciones para métricas (RF05) y tablero (CU06).  
- `IExportarReporte` → exportación de resultados (PDF/CSV) cuando aplica (extensión de CU05/CU06).  
- `IEtapaLectura` → obtener/listar etapas (usado por reportes/tablero).  
- `IEtapaEscritura` → crear/actualizar/asignar (usado por gestión de etapas CU02–CU04).

De esta manera:
- **Tablero** depende de `IReportesConsulta` (y opcionalmente `IEtapaLectura`), **sin** arrastrar exportación.
- **Servicio de Reportes** puede depender de `IReportesConsulta` y, solo si corresponde, de `IExportarReporte`.
- **Servicio de Etapas** depende de `IEtapaEscritura` (y `IEtapaLectura` si necesita validaciones previas).

## ¿Qué es una interfaz?

Una **interfaz** define un contrato: el *qué* debe poder hacer un objeto (métodos y sus firmas), sin imponer cómo lo hace (sin implementación).  
Características clave:
- **Sólo declara operaciones** (métodos públicos) y sus parámetros/retornos.
- **No contiene estado ni lógica** de negocio.
- **Permite múltiples implementaciones** intercambiables que cumplan el mismo contrato.

## Estructura de Clases (UML)
![Diagrama ISP](/diagramas/01-diagrama-clases/01-solid-04-isp.png)  
[Ver diagrama en detalle](/diagramas/01-diagrama-clases/01-solid-04-isp.puml)

## Justificación técnica
- Eliminamos **interfaces gordas**: cada cliente programa contra **lo que usa**.  
- Disminuye el **acoplamiento** y mejora la **testabilidad** (dobles de `IReportesConsulta` o `IEtapaLectura` sin arrastrar escritura ni exportación).  
- Mantiene trazabilidad con **RF02/RF05/RF06** y **CU02–CU06**.