# Pale Raider Goblin + IA Básica

**Versión:** 1.0.0
**Publisher:** Meshborn
**Engines compatibles:** Unity 6.0+ (URP/Built-in/HDRP) y Unreal Engine 5.4+

Un enemigo goblin stylized con IA integrada listo para usar. Un solo pack, dos engines. Máquina de estados en C# para Unity, Behavior Tree para Unreal. Sin configuración: arrastra el prefab/blueprint a tu escena y funciona.

---

## Contenido

- **1 goblin stylized low-poly** — 3,056 tris, atlas hand-painted 2K, un solo material.
- **1 lanza** — modelada por separado, anclada en la mano derecha.
- **Rig humanoide** — compatible con Mecanim (Unity), retargetable al mannequin de UE5.
- **4 animaciones** — Idle, Walk, Attack, Death. Idle y Walk con loop limpio. Attack y Death one-shot.
- **AI Controller para Unity (C#)** — FSM con estados Idle, Patrol, Chase, Attack, Dead. 3 scripts, sin dependencias.
- **AI para Unreal (Blueprint)** — BP_GoblinAI con Behavior Tree + Blackboard, BP_PatrolPath como actor.
- **2 escenas de demostración** — una por engine, con player placeholder y 1 goblins patrullando.

---

## Requisitos

### Unity
- Unity 6.0 LTS o superior (probado en Unity 6.4)
- Render pipeline: URP, Built-in o HDRP
- NavMesh horneado en tu escena
- Un GameObject con tag `Player` para que la IA lo detecte

### Unreal Engine
- UE 5.4 o superior (probado en UE 5.7)
- Una clase de Character/Pawn por defecto para el jugador
- Nav Mesh Bounds Volume cubriendo el área jugable

---

## Inicio rápido — Unity

1. Arrastra `Goblin.prefab` desde `Meshborn/PaleRaiderGoblin/Prefabs/` a tu escena.
2. Añade un componente `NavMeshSurface` a tu geometría y hornea el NavMesh (Window → AI → Navigation).
3. (Opcional) Arrastra un GameObject vacío con `GoblinPatrolPath` y rellena el array `Waypoints`.
4. Selecciona el goblin y asigna el Patrol Path en el campo `Patrol Path` del componente `GoblinAIController`.
5. Asegúrate de que tu jugador tenga el tag `Player`.
6. Pulsa Play. El goblin entra en idle, patrulla si tiene path, persigue al detectar, ataca en rango y muere cuando `TakeDamage(int)` baja su HP a 0.

Para infligir daño desde tu código:

```csharp
using Meshborn.PaleRaiderGoblin;

GetComponent<GoblinAIController>().TakeDamage(10);
```

---

## Inicio rápido — Unreal Engine

1. Arrastra `BP_Goblin` desde `Meshborn/PaleRaiderGoblin/Blueprints/` a tu nivel.
2. Coloca un actor `BP_PatrolPath` y añade waypoints en el array `Waypoints` (en el panel Details).
3. En la instancia del goblin, asigna la referencia al `BP_PatrolPath`.
4. Coloca un `Nav Mesh Bounds Volume` cubriendo el área jugable.
5. Pulsa Play. El goblin usa su Behavior Tree para patrullar, perseguir, atacar y morir.

Para infligir daño desde Blueprint:

```
Get BP_Goblin reference → Apply Damage (target=goblin, amount=10)
```

---

## Referencia de configuración

### GoblinAIController (Unity) / BP_Goblin (Unreal)

| Campo | Default | Descripción |
|---|---|---|
| Max HP | 30 | Vida del goblin |
| Attack Damage | 10 | Daño por golpe |
| Attack Cooldown | 1.2 s | Tiempo mínimo entre ataques |
| Idle Duration | 2 s | Tiempo en idle antes de retomar patrullaje |

### Sense Component (Unreal)

| Campo | Default | Descripción |
|---|---|---|
| View Distance | 10 m | Radio de detección |
| View Angle | 90° | Ángulo del cono de visión |
| Attack Range | 1.8 m | Distancia para entrar en estado Attack |
| Memory Duration | 3 s | Tiempo recordando al jugador tras perderlo de vista |
| Player Layer | (configurable) | Layer del jugador |
| Obstacle Layer | (configurable) | Layers que bloquean línea de visión |

---

## Cómo

### Spawnear un goblin en runtime (Unity)
```csharp
GameObject goblin = Instantiate(goblinPrefab, spawnPos, Quaternion.identity);
goblin.GetComponent<GoblinAIController>().enabled = true;
```

### Personalizar stats por instancia
Sobrescribe los campos directamente en cada instancia desde el Inspector (Unity) o el panel Details (Unreal). Todas las configuraciones son por instancia, no globales.

### Sustituir animaciones
Cambia los clips en el Animator Controller (Unity) o Animation Blueprint (Unreal). Mantén los nombres de estados y triggers para preservar las transiciones.

### Añadir audio
El pack se distribuye sin audio. Añade un AudioSource al prefab y llama a `audioSource.Play()` desde los callbacks de estado (Unity) o vía Notify Events en los tracks de animación (Unreal).

### Cambiar el aspecto del goblin
El modelo usa un único atlas hand-painted 2K. Edita `T_Goblin_BC.png` y reimporta. El layout UV se conserva entre reskins.

---

## Preguntas frecuentes

**¿Funciona en 2D?**
No. El pack depende de NavMeshAgent (Unity) / NavMeshBoundsVolume (UE) y un modelo 3D riggeado.

**¿Puedo retargetear el rig a mi propio personaje?**
Sí. El rig sigue la spec Humanoid de Mecanim en Unity y es retargetable al mannequin de UE5.

**¿Soporte multijugador?**
No de forma nativa. La lógica está pensada para server-authoritative — puedes correr la IA en el servidor y replicar transforms, pero la capa de networking la pones tú.

**¿Puedo vender un juego hecho con este asset?**
Sí. Los términos de licencia permiten uso comercial. Ver LICENSE.md.

**¿Habrá más goblins / variantes?**
Sí, más enemigos cada semana desde Meshborn. Misma familia de rig, mismo patrón drop-in.

---

## Soporte

- **Email:** meshborn.dev@gmail.com
- **Twitter / X:** [@meshborn](https://twitter.com/meshborn)
- **ArtStation:** [meshborn](https://artstation.com/meshborn)

Para bugs y preguntas, prefiere el email. Las issues reportadas en reseñas del marketplace se atienden pero el email privado tiene respuesta más rápida.

---

## Licencia

Ver `LICENSE.md`. El pack se vende bajo los términos de licencia de usuario final estándar del marketplace donde se compró (Fab o Unity Asset Store).

---

## Créditos

Ver `CREDITS.md`. Hecho por Meshborn. © 2026.
