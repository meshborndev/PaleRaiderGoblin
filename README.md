# Pale Raider Goblin + Basic AI

**Version:** 1.0.0
**Publisher:** Meshborn
**Engines:** Unity 6.0+ (URP/Built-in/HDRP) and Unreal Engine 5.4+

A drop-in stylized goblin enemy with built-in AI. One pack, two engines. C# state machine for Unity, Behavior Tree for Unreal — no setup beyond dragging the prefab/blueprint into your scene.

---

## Contents

- **1 stylized low-poly goblin** — 3,056 tris, hand-painted 2K texture atlas, single material.
- **1 spear prop** — modeled separately, attached to the right hand.
- **Humanoid rig** — Mecanim-compatible (Unity), retargetable to UE5 mannequin.
- **4 animations** — Idle, Walk, Attack, Death. Idle and Walk loop cleanly. Attack and Death are one-shot.
- **Unity AI Controller (C#)** — FSM with Idle, Patrol, Chase, Attack, Dead states. 3 scripts, no dependencies.
- **Unreal AI (Blueprint)** — BP_GoblinAI with Behavior Tree + Blackboard, BP_PatrolPath actor.
- **2 demo scenes** — one per engine, with player placeholder + 1 patrolling goblins.

---

## Requirements

### Unity
- Unity 6.0 LTS or newer (tested on Unity 6.4)
- Render pipeline: URP, Built-in, or HDRP
- NavMesh baked in your scene
- A GameObject tagged `Player` for detection

### Unreal Engine
- UE 5.4 or newer (tested on UE 5.7)
- A default character/pawn class for the player
- Navigation Mesh Bounds Volume covering the playable area

---

## Quick Start — Unity

1. Drag `Goblin.prefab` from `Meshborn/PaleRaiderGoblin/Prefabs/` into your scene.
2. Add a `NavMeshSurface` component to your level geometry and bake the NavMesh (Window → AI → Navigation).
3. (Optional) Drag a `GoblinPatrolPath` empty GameObject into the scene and fill its `Waypoints` array with Transforms.
4. Select the Goblin instance and assign the Patrol Path to the `GoblinAIController` component's `Patrol Path` field.
5. Make sure your player GameObject has the tag `Player`.
6. Press Play. The goblin idles, patrols if it has a path, chases on sight, attacks in range, dies when `TakeDamage(int)` brings HP to 0.

To deal damage to the goblin from your code:

```csharp
using Meshborn.PaleRaiderGoblin;

GetComponent<GoblinAIController>().TakeDamage(10);
```

---

## Quick Start — Unreal Engine

1. Drag `BP_Goblin` from `Meshborn/PaleRaiderGoblin/Blueprints/` into your level.
2. Drag a `BP_PatrolPath` actor into the scene and add waypoints in its `Waypoints` array (in the details panel).
3. On the Goblin instance, set the `Patrol Path` reference to the BP_PatrolPath actor.
4. Make sure your level has a `Nav Mesh Bounds Volume` covering the playable area.
5. Press Play. The goblin uses its Behavior Tree to patrol, chase, attack, and die.

To deal damage from a Blueprint:

```
Get BP_Goblin reference → Apply Damage (target=goblin, amount=10)
```

---

## Configuration Reference

### GoblinAIController (Unity) / BP_Goblin (Unreal)

| Field | Default | Description |
|---|---|---|
| Max HP | 30 | Goblin health points |
| Attack Damage | 10 | Damage dealt per attack swing |
| Attack Cooldown | 1.2 s | Minimum time between attacks |
| Idle Duration | 2 s | Time idling before resuming patrol |

### Sense Component (Unreal)

| Field | Default | Description |
|---|---|---|
| View Distance | 10 m | Detection radius |
| View Angle | 90° | FOV cone angle |
| Attack Range | 1.8 m | Distance to switch from Chase to Attack |
| Memory Duration | 3 s | Time after losing sight before giving up |
| Player Layer | (set by you) | Which layer the player is on |
| Obstacle Layer | (set by you) | Which layers block line of sight |

---

## How To

### Spawn a goblin at runtime (Unity)
```csharp
GameObject goblin = Instantiate(goblinPrefab, spawnPos, Quaternion.identity);
goblin.GetComponent<GoblinAIController>().enabled = true;
```

### Customize stats per instance
Override fields directly on each instance via the Inspector (Unity) or Details panel (Unreal). All settings are per-instance, not shared globals.

### Replace animations
Swap clips in the Animator Controller (Unity) or Animation Blueprint (Unreal). The state names are: Idle, Walk, Attack, Death. Use the same trigger/parameter names to keep transitions intact.

### Add audio
The pack ships without audio cues. Add an AudioSource to the prefab and call `audioSource.Play()` from inside the relevant state callbacks (Unity) or via Notify Events on the animation tracks (Unreal).

### Reskin the goblin
The model uses a single 2K hand-painted atlas. Edit `Goblin_TXT.png` and re-import. The UV layout is preserved across reskins.

---

## FAQ

**Q: Does it work in 2D?**
A: No. The pack relies on NavMeshAgent (Unity) / NavMeshBoundsVolume (UE) and a 3D rigged model.

**Q: Can I retarget the rig to my own character?**
A: Yes. The rig follows Mecanim's Humanoid spec in Unity, and is retargetable to the UE5 mannequin in Unreal.

**Q: Multiplayer support?**
A: Not native. The logic is server-authoritative friendly — you can run AI on the server and replicate transforms, but you'll need to wire your own networking layer.

**Q: Can I sell a game made with this asset?**
A: Yes. License terms allow commercial use. See LICENSE.md.

**Q: Will there be more goblins / variants?**
A: Yes, more enemies coming weekly from Meshborn. Same rig family, same drop-in pattern.

---

## Support

- **Email:** meshborn.dev@gmail.com
- **Twitter / X:** [@meshborn](https://twitter.com/meshborn)
- **ArtStation:** [meshborn](https://artstation.com/meshborn)

For bugs and questions, please use email. Issues reported in marketplace reviews will be addressed but private email gets faster turnaround.

---

## License

See `LICENSE.md`. The pack is sold under the standard end-user license of the marketplace where you purchased it (Fab or Unity Asset Store).

---

## Credits

See `CREDITS.md`. Made by Meshborn. © 2026.
