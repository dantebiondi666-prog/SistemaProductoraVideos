# Matriz CLAE (CRUD) – [Cambiar Estado Etapa #04]

**Proyecto:** Sistema de Gestión de Proyectos Audiovisuales  
**Caso de Uso:** [CU4 - Cambiar Estado Etapa]  

---

## 1) Tabla CLAE

> **Estructura:**  
> - **Filas:** Actividades internas del caso de uso (acciones o pasos del flujo).  
> - **Columnas:** Clases del sistema involucradas.  
> - **Celdas:** Letras **C**, **L**, **A**, **E** según la operación que se realiza sobre la clase.  
> - Si no aplica, dejar la celda vacía.  

| Actividad / Clase | Proyecto | Etapa | Usuario | ArchivoMultimedia | Notificación | ... |
|--------------------|:--------:|:-----:|:-------:|:-----------------:|:-------------:|-----:|
| Registrar nuevo proyecto | C |   | L |   |   |  |
| Agregar etapa inicial | L | C |   |   |   |  |
| Asignar responsable | A | L | L |   |   |  |
| Adjuntar archivos | L |   |   | C |   |  |
| Notificar creación | L |   |   |   | C |  |

> **Leyenda:**  
> **C**: Crear – **L**: Leer/Listar – **A**: Actualizar – **E**: Eliminar  

---

## 2) Métodos identificados

> Los métodos se derivan directamente de las operaciones (C/L/A/E) marcadas en la tabla.  
> Cada método deberá existir en la clase correspondiente del **diagrama de clases**, y reflejarse en su **Tarjeta CRC** y en el **diagrama de secuencia** del caso de uso.

| Clase | Método | Tipo (C/L/A/E) | Parámetros (nombre: tipo) | Retorno | Actividad asociada |
|--------|---------|----------------|----------------------------|----------|--------------------|
| Proyecto | `crearProyecto(nombre: string, fechaInicio: date)` | C | `nombre: string`, `fechaInicio: date` | `Proyecto` | Registrar nuevo proyecto |
| Etapa | `crearEtapa(proyectoId: int, nombre: string)` | C | `proyectoId: int`, `nombre: string` | `Etapa` | Agregar etapa inicial |
| Proyecto | `asignarResponsable(id: int, usuarioId: int)` | A | `id: int`, `usuarioId: int` | `bool` | Asignar responsable |
| ArchivoMultimedia | `subirArchivo(proyectoId: int, archivo: File)` | C | `proyectoId: int`, `archivo: File` | `bool` | Adjuntar archivos |
| Notificación | `enviarNotificacion(proyectoId: int, mensaje: string)` | C | `proyectoId: int`, `mensaje: string` | `bool` | Notificar creación |

---

## 3) Relación con otros artefactos del diseño

> Esta sección documenta la **trazabilidad** del caso de uso con los demás artefactos del modelo.  
> Cada fila establece una correspondencia entre los elementos de la matriz CLAE y los artefactos donde aparecen.

| Elemento | Artefacto vinculado | Archivo / Referencia URL | Descripción de la relación |
|-----------|--------------------|-----------------------|-----------------------------|
| `crearProyecto()` | Tarjeta CRC – Proyecto | [`herramientas-agile/tarjetas-crc/crc-proyecto.md`]() | Figura como responsabilidad “Registrar nuevo proyecto”. |
| `crearProyecto()` | Diagrama de Secuencia – CU1 | [`diagramas/05-diagramas-secuencia/05-secuencia-crear-proyecto.puml`]() | Aparece como mensaje enviado desde el actor “Productor” al objeto `Proyecto`. |
| `crearProyecto()` | Diagrama de Actividad – CU1 | [`diagramas/04-diagramas-actividades/04-actividad-crear-proyecto.puml`]() | Acción “Registrar nuevo proyecto” coincide con la operación **C**. |
| `asignarResponsable()` | Tarjeta CRC – Proyecto | [`crc-proyecto.md`]() | Declarado como método dentro de las responsabilidades del proyecto. |
| `subirArchivo()` | Diagrama de Secuencia – CU1 | [`05-secuencia-crear-proyecto.puml`]() | Se muestra como mensaje entre `Usuario` y `ArchivoMultimedia`. |
| `enviarNotificacion()` | Diagrama de Actividad – CU1 | [`04-actividad-crear-proyecto.puml`] | Acción final del flujo que notifica al responsable. |

---

## 4) Issues e inconsistencias detectadas

> Registrar cualquier diferencia encontrada entre esta matriz y los artefactos relacionados.

| URL | Descripción de la inconsistencia | Artefacto relacionado | Acción correctiva | Estado |
|----|----------------------------------|------------------------|-------------------|:------:|
| [#45](https://github.com/tu-org/tu-repo/issues/45) | `crearEtapa()` no figura en la CRC de Etapa | Tarjeta CRC – Etapa | Agregar responsabilidad y actualizar CRC | Pendiente |
| [#46](https://github.com/tu-org/tu-repo/issues/46) | Acción “Notificar creación” no aparece en el diagrama de secuencia | Diagrama de Secuencia – CU1 | Incorporar mensaje a `Notificación` | Pendiente |
| [#46](https://github.com/tu-org/tu-repo/issues/45)| Método `subirArchivo()` sin tipo de retorno en diagrama de clases | Diagrama de Clases | Agregar tipo `bool` en puml | Resuelto |

**Estados posibles:** Abierto / Pendiente / Resuelto

---

