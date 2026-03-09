# Turret

<figure><img src="../../.gitbook/assets/Turret.png" alt=""><figcaption></figcaption></figure>

## Introduction

A Turret is a **programmable structure** in space that projects offensive or defensive power over a fixed location. It is anchored on a base (grid), draws power from the network node, and automatically engages targets within its proximity. The game continuously evaluates nearby entities and asks *which* target to shoot and in what order—either using built-in default logic or your custom extension.

### Default Behavior

By default (no extension configured), the game uses the built-in logic in the [world turret contract](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/assemblies/turret.move) to decide which targets to shoot. Default rules:

- **Exclude** — same tribe as owner and not an aggressor; or target **stopped** attacking
- **Prioritize** — **started attacking** adds a large weight increment; **entered** proximity adds a smaller weight if different tribe or aggressor
- The game shoots the target with the highest priority weight; ties use list order.

### Custom Behavior (Extension)

When the owner configures an extension, the game uses that extension’s logic instead of the default to decide which targets to shoot and in what order. The extension receives the same target information the game uses and returns a custom priority list.

1. The owner deploys custom targeting logic (a [Move contract](https://github.com/evefrontier/world-contracts/blob/main/contracts/extension_examples/sources/turret.move)) and registers it as the turret’s extension.
2. Whenever the game evaluates targets, it calls that logic and uses the returned list to choose what to shoot.

## Behaviours

The game calls your extension **whenever target behaviour changes** (e.g. a ship enters range, starts or stops attacking). It sends **exactly one behaviour change per candidate**; if both ENTERED and STARTED_ATTACK apply, the game sends STARTED_ATTACK (the higher-priority). Your extension returns which targets to shoot and at what priority weight; the game shoots the highest-weight target, and uses list order to break ties.

**Behaviour reasons:**

- **ENTERED** — target entered the turret’s proximity
- **STARTED_ATTACK** — target started attacking the base (or someone on grid)
- **STOPPED_ATTACK** — target stopped attacking

**Target candidate and return structs** (from the [world turret module](https://github.com/evefrontier/world-contracts/blob/3c35d6dce5f107bb8dd286e589330929ac401423/contracts/world/sources/assemblies/turret.move#L82)):

```move
public struct TargetCandidate has copy, drop, store {
    item_id: u64,
    type_id: u64,
    group_id: u64,
    character_id: u32,
    character_tribe: u32,
    hp_ratio: u64,
    shield_ratio: u64,
    armor_ratio: u64,
    is_aggressor: bool,
    priority_weight: u64,
    behaviour_change: BehaviourChangeReason,
}

public struct ReturnTargetPriorityList has copy, drop, store {
    target_item_id: u64,
    priority_weight: u64,
}
```

**TargetCandidate field meanings:**

| Field | Meaning |
|-------|---------|
| `item_id` | Unique id for the target (ship_id / npc_id). |
| `type_id` | Target type (ship or NPC). |
| `group_id` | Ship group; helps prioritize by ship class. 0 for NPCs. |
| `character_id` | Pilot character id; 0 for NPCs. |
| `character_tribe` | Character tribe; 0 for NPCs. |
| `hp_ratio`, `shield_ratio`, `armor_ratio` | Remaining structure/shield/armor as percentage (0–100). |
| `is_aggressor` | True if this target is attacking anyone on grid (structure or player). |
| `priority_weight` | Default weight from game; your extension can override via the return list. |
| `behaviour_change` | The single reason sent for this candidate (e.g. STARTED_ATTACK over ENTERED when both apply). |

**Return list:** the game shoots the target with the highest `priority_weight` in your returned list; if equal, it uses the order in the list.

### Ship groups and turret specialization

Turret types in the game are specialized against certain ship groups. You can use `group_id` to prefer targets your turret is good against:

| Ship class | group_id |
|------------|----------|
| Shuttle | 31 |
| Corvette | 237 |
| Frigate | 25 |
| Destroyer | 420 |
| Cruiser | 26 |
| Combat Battlecruiser | 419 |

| Turret type | type_id | Specialized against (group_ids) |
|-------------|---------|--------------------------------|
| Autocannon | 92402 | Shuttle (31), Corvette (237) |
| Plasma | 92403 | Frigate (25), Destroyer (420) |
| Howitzer | 92484 | Cruiser (26), Combat Battlecruiser (419) |

## Next Steps

Build and test a custom turret extension: [Build Guide](./build.md)

**Reference:**
- [world-contracts](https://github.com/evefrontier/world-contracts) — EVE Frontier Sui Move contracts
- [turret.move](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/assemblies/turret.move) — core turret module
- [extension_examples/turret.move](https://github.com/evefrontier/world-contracts/blob/main/contracts/extension_examples/sources/turret.move) — example custom turret extension
- [contracts/world](https://github.com/evefrontier/world-contracts/tree/main/contracts/world) — world contract package
