# Anexo - Introducción al Diseño Orientado a Objetos

El paradigma orientado a objetos (abreviado POO), es un modelo de programación que organiza y forma al sistema como un conjunto de objetos los cuales representan entidades del mundo real o conceptos abstractos. Los objetos poseen atributos (datos o propiedades) los cuales describen su estado y como es el objeto, y métodos (funcionalidades y comportamientos) los cuales definen que cosas puede hacer el objeto.
Una de las mayores ventajas del POO es que los programas se centran en clases (tipos) y objetos (instancias), encapsulando datos y operaciones para reducir complejidad y facilitar mantenimiento y reutilización.

## Los cuatro fundamentos del POO

### 1. Abstracción
**Concepto:** identificar y representar solo las características esenciales de un objeto del mundo real en el sistema, dejando de lado los detalles innecesarios para el contexto.

**Ejemplo:**  
En el sistema, un supuesto **ProyectoAudiovisual** se abstrae como un objeto con atributos como *nombre, cliente, fechas, etapas, estado*.  
No es necesario modelar detalles irrelevantes como el color del logo del cliente, el modelo de cámara usada en la grabación o la tipografía del contrato.  
La abstracción permite enfocarse en lo que es relevante para gestionar y dar seguimiento a un proyecto, sin cargar el sistema con datos irrelevantes que no aportan al objetivo.

---

### 2. Encapsulamiento
**Concepto:** proteger los datos de un objeto controlando el acceso mediante métodos.  

**Ejemplo:**  
Una supuesta clase **Etapa** guardaría los valores `estado` y `responsable`.  
Estos valores no tendrían que poder ser modificados directamente, salvo mediante métodos como `cambiarEstado()` o `asignarResponsable()`, asegurando consistencia.

---

### 3. Herencia
**Concepto:** crear nuevas clases a partir de otras, reutilizando atributos y métodos comunes.  

**Ejemplo:**  
Una clase **Usuario** general, de la cual heredan **Productor**, **Editor**, **Asistente**, y más adelante **Cliente**.  
Cada uno puede tener atributos y permisos adicionales.

---

### 4. Polimorfismo
**Concepto:** distintas clases pueden responder de manera diferente al mismo mensaje o método.  

**Ejemplo:**  
Un supuesto método `notificar()` puede enviar un **mail** si el responsable prefiere correo, o un **mensaje de WhatsApp** si así está configurado.  
Es el mismo método, pero distinto comportamiento según el objeto.

## Requisitos iniciales del sistema

### Requisitos funcionales:

- **RF01. Administración de proyectos:** El sistema debe permitir **crear y editar proyectos**, registrando al menos: nombre, cliente, fechas de inicio y fin estimadas, estado actual y observaciones.  

- **RF02. Gestión de etapas:** El sistema debe permitir **agregar y modificar etapas** en un proyecto, indicando responsable, estado (Pendiente, En curso, Finalizada), fechas estimadas y observaciones.

- **RF03. Tablero de control:** El sistema debe mostrar un **panel general** donde se visualice el estado de todos los proyectos y sus etapas, indicando si están en curso, finalizados o pausados y el responsable de cada etapa.
  
- **RF04. Notificaciones automáticas:** El sistema debe **enviar notificaciones automáticas** (por mail y WhatsApp) al responsable cuando:  
  - Se le asigna una nueva etapa.  
  - Cambia el estado de su etapa.  
  - Una etapa a su cargo pasa a “Finalizada”.

- **RF05. Registro estadístico:** El sistema debe permitir **consultar métricas** de proyectos y etapas, incluyendo:  
  - Cantidad de proyectos entregados por mes.  
  - Duración promedio de proyectos y etapas.  
  - Proyectos filtrados por cliente o tipo.
    
- **RF06. Historial de tareas:** El sistema debe **registrar automáticamente** qué usuario completó cada tarea y los cambios de estado de cada etapa.

### Requisitos no funcionales:

- **RNF01. Accesibilidad multiplataforma:** El sistema debe estar disponible desde **navegadores web de escritorio y móviles** (Chrome, Edge, Safari) y ser responsive en pantallas de al menos 5” de ancho.

- **RNF02. Facilidad de uso:** El sistema debe permitir que **un usuario nuevo realice su primera acción (crear proyecto o etapa)** en menos de **10 minutos de aprendizaje autónomo**, con menús y filtros accesibles.

- **RNF03. Disponibilidad:** El sistema debe estar disponible al menos el **99% del tiempo**, excluyendo mantenimientos planificados.

- **RNF04. Escalabilidad:** El sistema debe soportar la gestión de al menos **100 proyectos en simultáneo**, con posibilidad de extender a nuevas funciones como facturación sin rediseño completo.

- **RNF05. Guardado de datos:** El sistema debe guardar automáticamente cada cambio en proyectos y etapas en menos de **2 segundos**, garantizando persistencia aunque se recargue la página.

- **RNF06. Rendimiento:** El sistema debe permitir consultar y filtrar proyectos sin que el tiempo de respuesta supere **3 segundos** en escenarios con hasta **50 proyectos activos y 200 etapas**.

## Casos de uso

### CU01 – Crear/Editar Proyecto (RF01)
- **Actor principal:** Productor / Coordinador
- **Actores secundarios:** Servicio de Notificaciones, cliente/administrador  
- **Objetivo:** Registrar o actualizar un proyecto con sus datos mínimos y dejarlo disponible para seguimiento.  
- **Precondiciones:** Usuario autenticado con permiso; el nombre del proyecto no debe estar duplicado.  
- **Flujo principal:**  
  1. El actor navega a **Proyectos** y selecciona **Nuevo** o **Editar**.  
  2. El sistema muestra un **formulario** con campos: nombre, cliente, fechas de inicio y fin estimada, estado, observaciones.  
  3. El actor **completa** los campos obligatorios.  
  4. El sistema **valida** (obligatorios, formato de fechas, duplicados de nombre).  
  5. Si hay errores, el sistema **resalta** los campos inválidos y muestra mensajes.  
  6. El actor **corrige** y reenvía.  
  7. El sistema **persiste** el proyecto y registra auditoría (quién/cuándo).
  8. El **Servicio de Notificaciones** envía aviso de "proyecto creado/editado" al cliente y/o administrador.  
  9. El sistema **confirma** y redirige al detalle del proyecto.  
- **Flujos alternativos:** Campos obligatorios incompletos o nombre duplicado → rechazo con mensaje, volver a paso 3.  
- **Postcondiciones:** Proyecto persistido y disponible en listados/tablero.

---

### CU02 – Gestionar Etapas de un Proyecto (RF02)
- **Actor principal:** Coordinador
- **Actores secundarios:** Usuario responsable *(persona asignada a etapa)*, servicio de notificaciones.  
- **Objetivo:** Agregar, editar o eliminar etapas de un proyecto.  
- **Precondiciones:** Proyecto existente; actor con permisos.  
- **Flujo principal:**  
  1. El coordinador abre el **detalle del proyecto** y selecciona **Gestionar etapas**.  
  2. El sistema muestra la **lista de etapas** (si existen).  
  3. El coordinador elige **Agregar**, **Editar** o **Eliminar**.  
  4. El sistema despliega formulario con **nombre, responsable, estado, fechas estimadas, observaciones**.  
  5. El coordinador completa o modifica los campos y confirma.  
  6. El sistema **valida** (responsable válido, fechas coherentes, estado permitido).  
  7. El sistema **guarda** los cambios y registra el evento (alta/edición/baja).
  8. El **servicio de notificaciones** envía aviso al **usuario responsable** asignado afectado por la modificación.  
  9. El sistema **actualiza** la lista y muestra confirmación al coordinador.  
- **Flujos alternativos:** Responsable inexistente o fechas inválidas → rechazo con mensaje, volver a paso 4.  
- **Postcondiciones:** Etapas actualizadas y reflejadas en el tablero.

---

### CU03 – Asignar Responsable a Etapa (RF02, RF04, RF06)
- **Actor principal:** Coordinador
- **Actores secundarios:** usuario responsable, servicio de notificaciones.  
- **Objetivo:** Asignar o cambiar el responsable de una etapa y notificarlo.  
- **Precondiciones:** Etapa existente; usuario válido en el sistema, coordinador autenticado con permisos para asignar responsables.  
- **Flujo principal:**  
  1. El coordinador abre el **detalle** del proyecto y selecciona una etapa.  
  2. El sistema muestra los datos actuales de la etapa.  
  3. El coordinador selecciona en **Asignar/Cambiar responsable**.  
  4. El sistema despliega un **selector de usuarios**.  
  5. El coordinador elige al responsable y confirma.  
  6. El sistema valida el usuario y actualiza la etapa.  
  7. El sistema registra el cambio en el **historial** (quién, a quién, cuándo).  
  8. El **servicio de notificaciones** envía un aviso al **usuario responsable** indicando la asignación/cambio.
  9. El sistema muestra confirmación al coordinador.  
- **Flujos alternativos:** Usuario inactivo o sin permisos → rechazo.  
- **Postcondiciones:** Responsable asignado; notificación enviada a usuario responsable; historial actualizado.

---

### CU04 – Cambiar Estado de Etapa (RF02, RF04, RF06)
- **Actor principal:** Responsable de la etapa / Coordinador
- **Actores secundarios:** Servicio de notificaciones, coordinador (si el cambio lo hace el responsable de etapa).  
- **Objetivo:** Modificar el estado de una etapa respetando reglas de negocio establecidas.  
- **Precondiciones:** Etapa existente; usuario autenticado y con permisos.  
- **Flujo principal:**  
  1. El actor abre el detalle de la etapa.  
  2. El sistema muestra estado actual y opciones: Pendiente, En curso, Finalizada.  
  3. El actor selecciona un nuevo estado y confirma.  
  4. El sistema valida reglas (p.ej. no finalizar si tareas incompletas).  
  5. Si la validación falla, muestra error.  
  6. Si es correcta, el sistema **actualiza el estado de la etapa**.  
  7. El sistema registra el cambio en **historial**.  
  8. El **servicio de notificaciones** envía notificación a los interesados.
      - Si el cambio lo hizo el Responsable → se notifica al Coordinador.
      - Si el cambio lo hizo el Coordinador → se notifica al Responsable.
  9. El sistema muestra confirmación al actor principal.  
- **Flujos alternativos:** Reglas incumplidas → no actualizar y avisar.  
- **Postcondiciones:** Estado actualizado; historial y notificaciones registradas.

---

### CU05 – Consultar Reportes/Métricas (RF05, RNF06)
- **Actor principal:** Productor / Coordinador
- **Actor secundario:** servicio de exportación    
- **Objetivo:** Obtener métricas de proyectos y etapas con filtros.  
- **Precondiciones:** Existencia de datos; usuario con permisos.  
- **Flujo principal:**  
  1. El actor accede a **Reportes**.  
  2. El sistema muestra filtros (cliente, tipo, fechas).  
  3. El actor configura y aplica los filtros.  
  4. El sistema consulta datos y calcula métricas (proyectos finalizados por mes, duración promedio, tipo más frecuente).  
  5. El sistema muestra visualizaciones (tabla/gráfico).  
  6. El actor aplica filtros adicionales o cambia parámetros.  
  7. El sistema actualiza resultados en ≤3 segundos.  
  8. El actor exporta a PDF/CSV si desea.
  9. El **servicio de exportación** genera el archivo en el formato seleccionado y lo entrega al actor.  
- **Flujos alternativos:** Sin datos → mensaje “sin resultados”.  
- **Postcondiciones:** Reporte visualizado/exportado, archivo exportado a PDF/CSV disponible para descarga.

---

### CU06 – Mostrar Tablero de Control (RF03)
- **Actor principal:** Usuario autenticado
- **Actor secundario:** Servicio de exportación  
- **Objetivo:** Visualizar estado general de proyectos y etapas.  
- **Precondiciones:** Usuario autenticado; datos registrados.  
- **Flujo principal:**  
  1. El usuario abre **Tablero** desde menú.  
  2. El sistema carga el resumen de proyectos.  
  3. El sistema muestra tarjetas con estado, avance y responsables.  
  4. El usuario aplica filtros (estado, cliente, tipo).  
  5. El sistema actualiza resultados.  
  6. El usuario expande un proyecto para ver etapas.  
  7. El sistema muestra detalles de etapas.  
  8. El usuario selecciona exportar resumen.
  9. El **servicio de exportación** genera el archivo y lo deja disponible para descargar.
  10. El sistema confirma la acción. 
- **Flujos alternativos:** Sin proyectos → mensaje "no hay proyectos" y ofrece “Crear proyecto”.  
- **Postcondiciones:** Estado general visible; navegación habilitada, resumen exportado y disponible para descarga.

---

### CU07 – Enviar Notificaciones Automáticas (RF04, RF06)
- **Actor principal:** Coordinador/Responsable de etapa
- **Actores secundarios:** Servicio de notificaciones, destinatario  
- **Objetivo:** Avisar automáticamente a los usuarios responsables ante eventos relevantes en proyectos o etapas.  
- **Precondiciones:** Usuario con canal de notificación definido; conexión con servicio de notificaciones.  
- **Flujo principal:**  
  1. El Coordinador o Responsable realiza una acción relevante (ej.: asignar responsable, cambiar estado).  
  2. El sistema identifica destinatarios según el evento.  
  3. El sistema determina canal de notificación según preferencias de cada usuario.  
  4. El sistema compone mensaje con datos del proyecto/etapa.  
  5. El **servicio de notificaciones** envía la notificación al destinatario.  
  6. El servicio de notificaciones devuelve resultado (éxito/fallo).  
  7. El sistema registra el envío en historial de notificaciones.  
  8. Si falla, reintenta o marca la notificación para revisión.  
- **Flujos alternativos:** Usuario sin preferencia → canal por defecto; servicio de notificaciones caido → reintento.  
- **Postcondiciones:** Notificación registrada; destinatarios informados.

## Boceto inicial del diseño de clases

### Diagrama de Clases Iniciales

El siguiente diagrama corresponde al diseño de clases iniciales para el sistema:

![Diagrama de Clases](../diagramas/01-diagrama-clases/01-boceto-inicial.png)

[🔗 Ver diagrama en GitHub](../diagramas/01-diagrama-clases/01-boceto-inicial.png)