# Principio Abierto/Cerrado (OCP)

## Propósito y Tipo del Principio SOLID
El **Principio Abierto/Cerrado (OCP)** indica que los módulos deben estar **abiertos a extensión** pero **cerrados a modificación**.  
Tipo: **Extensión** (permite agregar nuevas capacidades sin tocar código estable).

## Motivación
En el diseño original, incorporar un nuevo **canal de notificación** o cambiar las **reglas de finalización de proyecto** obligaba a modificar clases existentes (acoplamiento alto, `if/switch` por tipo y riesgo de regresiones).  
Queremos sumar funcionalidades creando **nuevas clases**, sin editar las ya probadas.

## Explicación de Herencia (cómo aplicamos OCP)

Herencia significa que una clase hija toma todo lo que ya tiene una clase padre, lo puede usar y ampliar.

También se puede decir que **Herencia** es una relación de entre una clase base y sus subclases. La subclase **hereda** la interfaz y puede **especializar** comportamiento manteniendo la compatibilidad con el tipo base.

Es decir, Cuando necesito una variante, creo una subclase nueva y no toco la clase que ya existe.
La clase base define los métodos que todos deben tener.
Cada subclase agrega comportamiento sin modificar la clase base.
Así, el sistema queda abierto a extensión y cerrado a modificación.

Usamos **interfaces y jerarquías polimórficas** para definir puntos de extensión:
- **Strategy (Canales):** interfaz `CanalNotificacion` con implementaciones `CanalEmail`, `CanalWhatsApp`, `CanalPush`, `CanalSlack`.  
  `DespachadorNotificaciones` resuelve el `canalId` y delega el envío **sin** condicionales por tipo.

- **State (Etapas):** clase abstracta `EstadoEtapa` con subclases `Pendiente`, `EnCurso`, `Bloqueada`, `Finalizada`.  
  `Etapa` compone 1..1 un `estadoActual: EstadoEtapa`.

- **Specification (Proyecto):** interfaz `EspecificacionProyectoFinalizable` con implementaciones `EspecSinBloqueos`, `EspecAprobacionCliente`, `EspecEtapasFinalizadas` y el combinador `EspecificacionY`.

## Estructura de Clases
Diagrama de clases OCP (versión final):

![Diagrama UML - OCP](/diagramas/01-diagrama-clases/01-solid-02-ocp.png)

**Elementos clave**:
- `Proyecto.finalizarSiCorresponde(spec: EspecificacionProyectoFinalizable)`.
- `Etapa` → `estadoActual: EstadoEtapa` (composición 1..1) y `cambiarEstado(nuevo: EstadoEtapa)`.
- `DespachadorNotificaciones` → `CanalNotificacion` (registro y resolución).
- Implementaciones de `CanalNotificacion`: `CanalEmail`, `CanalWhatsApp`, `CanalPush`, `CanalSlack`.
- Implementaciones de `EspecificacionProyectoFinalizable`: `EspecSinBloqueos`, `EspecAprobacionCliente`, `EspecEtapasFinalizadas`, `EspecificacionY`.

## Justificación Técnica
- **Canales:** añadir un canal = crear una clase que implementa `CanalNotificacion` y registrarla en `DespachadorNotificaciones`. No se tocan `Notificacion`, `Usuario` ni el despachador → **cumple OCP**.
- **Estados:** añadir un estado = nueva subclase de `EstadoEtapa`. `Etapa` permanece cerrada a cambios → **cumple OCP**.
- **Reglas del proyecto:** nuevos criterios = nuevas `Especificacion*` o combinaciones con `EspecificacionY`. `Proyecto` no se modifica → **cumple OCP**.
- **Resultado:** menos acoplamiento, eliminación de condicionales por tipo y pruebas unitarias claras (cada Strategy/State/Spec se testea en forma aislada).

Aclaración sobre los patrones Strategy, State y Specification:

**Strategy**, elige cómo hacer algo.
  **Aplicación**: CanalNotificacion + CanalEmail/WhatsApp/Push/Slack y DespachadorNotificaciones que selecciona y delega. Agregar un canal no modifica el resto → OCP

**Specification**, evalúa reglas y permite componer condiciones.
  **Aplicación**: Etapa compone estadoActual: EstadoEtapa; las subclases definen reglas de transición. Agregar un estado no cambia Etapa → OCP.

**State**, cambia el comportamiento según el estado.
  **Aplicación**: EspecificacionProyectoFinalizable (interfaz) y sus implementaciones (EspecSinBloqueos, EspecAprobacionCliente, EspecificacionY,EspecEtapasFinalizadas). Proyecto recibe una especificacion y decide. Nuevas reglas = nuevas clases → OCP.