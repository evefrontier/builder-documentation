# Gate

<figure><img src="../../.gitbook/assets/Gate.png" alt=""><figcaption></figcaption></figure>

## Introduction

A Gate is a structure in space that enables travel between locations. Two gates link together to create a transport route. Gates are **programmable** — the owner can deploy custom extension contracts to control who can jump.

### Default Behavior

By default (no extension configured), **anyone can jump** through the gate without restrictions.

### Custom Behavior (Extension)

When the owner configures an extension, the gate switches to a **permit-based** model:

1. The owner deploys custom jump rules (a [Move contract](https://github.com/evefrontier/world-contracts)) and registers it as the gate’s extension.
2. Players must obtain a **jump permit** from that logic (e.g. by meeting a tribe check, paying a toll, or completing a bounty) before they can travel.
3. The game validates the permit and allows the jump. Permits are tied to a route and an expiry time; a permit for Gate A ↔ Gate B works in both directions.

For a full working example, see [builder-scaffold smart_gate](https://github.com/evefrontier/builder-scaffold/tree/main/move-contracts/smart_gate).

## Linking Gates

Two gates must be **linked** before anyone can jump between them. Requirements:

- Both gates must be owned by the same character.
- Gates must be at least 20 km apart (verified with a server-signed distance proof).
- Linking requires an authorized transaction (the game validates this).

## Next Steps

Build and test a custom smart gate end-to-end: [Build Guide](./build.md)

**Reference:**
- [world-contracts](https://github.com/evefrontier/world-contracts) — EVE Frontier Sui Move contracts
- [gate.move](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/assemblies/gate.move) — core gate module
- [builder-scaffold smart_gate](https://github.com/evefrontier/builder-scaffold/tree/main/move-contracts/smart_gate) — example gate extension
- [contracts/world](https://github.com/evefrontier/world-contracts/tree/main/contracts/world) — world contract package
