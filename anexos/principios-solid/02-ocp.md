# Principio Abierto/Cerrado (OCP)

## Propósito y Tipo del Principio SOLID
El **Principio Abierto/Cerrado (OCP)** indica que los módulos deben estar **abiertos a extensión** pero **cerrados a modificación**.  
Tipo: **Extensión** (permite agregar nuevas capacidades sin tocar código estable).

## Motivación
En el diseño original, incorporar un nuevo **canal de notificación** o cambiar las **reglas de finalización de proyecto** obligaba a modificar clases existentes (acoplamiento alto, `if/switch` por tipo y riesgo de regresiones).  
Queremos sumar funcionalidades creando **nuevas clases**, sin editar las ya probadas.

## Explicación de Herencia (cómo aplicamos OCP)
Usamos **interfaces y jerarquías polimórficas** para definir puntos de extensión:
- **Strategy (Canales):** interfaz `CanalNotificacion` con implementaciones `CanalEmail`, `CanalWhatsApp`, `CanalPush`, `CanalSlack`.  
  `DespachadorNotificaciones` resuelve el `canalId` y delega el envío **sin** condicionales por tipo.
- **State (Etapas):** clase abstracta `EstadoEtapa` con subclases `Pendiente`, `EnCurso`, `Bloqueada`, `Finalizada`.  
  `Etapa` compone 1..1 un `estadoActual: EstadoEtapa`.
- **Specification (Proyecto):** interfaz `EspecificacionProyectoFinalizable` con implementaciones `EspecSinBloqueos`, `EspecAprobacionCliente`, `EspecEtapasFinalizadas` y el combinador `EspecificacionY`.

## Estructura de Clases
Diagrama de clases OCP (versión final):

![Diagrama UML - OCP](/diagramas/01-diagrama-clases/01-solid-03-ocp.png)

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