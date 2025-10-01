# Principio de Sustitución de Liskov (LSP)

## Propósito y Tipo del Principio SOLID
El **Principio de Sustitución de Liskov (LSP)** establece que las subclases deben poder **sustituir** a sus superclases **sin alterar** el comportamiento esperado por los clientes.  
Tipo: **Extensión / Correctitud de jerarquías** (garantiza polimorfismo seguro).

## Motivación
En versiones previas, ciertos roles o canales requerían condicionales y algunas operaciones podían resultar “no soportadas”, rompiendo la sustituibilidad (por ejemplo, listas de `Usuario` que fallan con un rol específico).  
Con LSP, todas las subclases **cumplen el mismo contrato** y pueden usarse indistintamente donde se espera la superclase.

## Explicación de Herencia
- **Herencia**: una subclase extiende a la superclase heredando su **contrato público**.  
- Para cumplir LSP, las subclases **no deben** reforzar precondiciones, relajar postcondiciones ni romper invariantes del tipo base, ni lanzar excepciones por operaciones “no soportadas”.

Aplicación en el sistema:
- `Usuario` es **abstracta**; `Admin`, `Coordinador`, `Colaborador` respetan el mismo contrato (`puedeGestionarProyectos`, `puedeGestionarEtapas`, `notificar`).
- **Canales**: la interfaz `CanalNotificacion` unifica el contrato; `CanalEmail`, `CanalWhatsApp`, `CanalPush` y el legado `ServicioNotificaciones` lo implementan. Los clientes dependen de la **abstracción**.
- **Estados de etapa**: `EstadoEtapa` define el contrato; subclases (`Pendiente`, `EnCurso`, `Finalizada`) aplican reglas sin romper la API de `Etapa`.

## Estructura de Clases
Diagrama de clases LSP (versión final):

![Diagrama UML - LSP](/diagramas/01-diagrama-clases/01-solid-03-lsp.png)

**Elementos clave**:
- **Usuarios**: `Usuario` (abstracta) ← `Admin`, `Coordinador`, `Colaborador`. Misma interfaz pública; sin métodos “no soportados”.
- **Canales**: `CanalNotificacion` implementado por `CanalEmail`, `CanalWhatsApp`, `CanalPush` y `ServicioNotificaciones`. Los clientes dependen de la **abstracción**.
- **Estados**: `Etapa` compone `estadoActual: EstadoEtapa` (1..1) y expone `cambiarEstado(nuevo: EstadoEtapa)`. Los estados operan sobre su **contexto** (`EstadoEtapa ..> Etapa: contexto`).

## Justificación Técnica
- **Sustituibilidad de Usuario**: cualquier instancia (`Admin`, `Coordinador`, `Colaborador`) funciona donde se espera `Usuario` porque conserva el contrato público → **cumple LSP**.
- **Sustituibilidad de Canales**: todas las implementaciones comparten la firma `enviar(n: Notificacion): ResultadoEnvio`; se pueden reemplazar sin cambios en los clientes → **cumple LSP**.
- **Sustituibilidad de Estados**: cada subclase de `EstadoEtapa` respeta los mismos métodos y pre/postcondiciones. `Finalizada` restringe acciones de forma controlada sin romper la API → **cumple LSP**.
- **Resultado**: jerarquías coherentes, sin `instanceof`/`switch` por tipo y con pruebas de contrato reutilizables para cada subclase.