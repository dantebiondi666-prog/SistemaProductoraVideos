# Matriz CLAE (CRUD) – CU06 Mostrar Tablero de Control

**Proyecto:** Sistema de Gestión de Proyectos Audiovisuales  
**Caso de Uso:** CU06 – Mostrar Tablero de Control (RF03)

---

## 1) Tabla CLAE

> CU06 es **sólo de lectura**: no crea ni actualiza entidades.  
> Se consultan Proyectos, sus Etapas y el Usuario autenticado para construir el tablero.

**Columnas:**
- **Pry** = Proyecto
- **Etp** = Etapa
- **Usr** = Usuario

| Actividad funcional                                       | Pry | Etp | Usr |
|-----------------------------------------------------------|:---:|:---:|:---:|
| Cargar resumen inicial del tablero                        |  L  |  L  |  L  |
| Aplicar filtros (estado / cliente / fechas)               |  L  |  L  |  L  |
| Expandir proyecto y ver detalle de etapas                 |  L  |  L  |     |
| Ver totales y KPI provenientes de datos existentes        |  L  |  L  |     |

**Leyenda:** **C** Crear – **L** Leer/Listar – **A** Actualizar – **E** Eliminar

---

## 2) Métodos identificados (sin introducir nuevas clases)

> Se reutilizan consultas del **dominio** (o del repositorio si lo tenés separado). Ajustá nombres a tu diagrama final.

| Clase    | Método / Consulta sugerida                        | Tipo | Parámetros                              | Retorno          | Nota |
|----------|----------------------------------------------------|:---:|------------------------------------------|------------------|------|
| Proyecto | listarPorFiltros(estado?, cliente?, rangoFechas?) |  L  | filtros                                  | List\<Proyecto>  | Para armar el grid principal. |
| Proyecto | obtenerPorId(idProyecto: UUID)                    |  L  | idProyecto                               | Proyecto         | Para expandir detalle. |
| Etapa    | listarPorProyecto(idProyecto: UUID)               |  L  | idProyecto                               | List\<Etapa>     | Detalle de etapas. |
| Usuario  | obtenerPorId(idUsuario: UUID)                     |  L  | idUsuario                                | Usuario          | Contexto de usuario/logueo. |

---

## 3) Trazabilidad

| Elemento / Paso                        | Artefacto vinculado                 | Archivo / Referencia                                                                                                                                               | Descripción                                  |
|---------------------------------------|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------|
| Cargar resumen / aplicar filtros       | Diagrama de Actividad – CU06        | [04-actividad-mostrar-tablero-control-06.puml](../../diagramas/04-diagramas-actividades/04-actividad-mostrar-tablero-control-06.puml)                             | Pasos “cargar resumen” y “aplicar filtros”.  |
| Expandir proyecto / ver etapas         | Diagrama de Secuencia – CU06        | [05-secuencia-caso-uso-06-mostrar-tablero-escenario-06.puml](../../diagramas/05-diagramas-secuencia/05-secuencia-caso-uso-06-mostrar-tablero-escenario-06.puml)                                                                                | Mensajes Pantalla ↔ dominio.                 |
| Lectura de Proyecto/Etapa/Usuario      | Diagrama de Clases                  | [01-diagrama-clases-final.puml](../../diagramas/01-diagrama-clases/01-diagrama-clases-final.puml)                                                                                                   | Se leen entidades del dominio.               |

> Si preferís, reemplazá las rutas por **URLs del repo** (GitHub/GitLab) para la corrección docente.

---

## 4) Issues e inconsistencias detectadas

| URL / referencia | Descripción                                                                                                          | Artefacto relacionado | Acción correctiva                                                                                       | Estado     |
|------------------|----------------------------------------------------------------------------------------------------------------------|-----------------------|----------------------------------------------------------------------------------------------------------|-----------|
| *Este archivo*   | Se habían referido clases no modeladas (ServicioTablero, MotorTablero, CacheTablero, ServicioExportación, Archivo…). | Matriz CLAE CU06      | **Eliminar** esas referencias y ceñirse a Proyecto/Etapa/Usuario.                                        | **Resuelto** |
| `diagsecuencia06`| Si el diagrama aún invoca “servicios” no definidos en clases/CRCs.                                                   | Diagrama de Secuencia | Ajustar mensajes para que consulten **métodos/DAOs** del dominio (o agregar esas interfaces al UML).     | Pendiente  |