# EVE Frontier World Explainer

<figure><img src="../.gitbook/assets/banner4.png" alt=""><figcaption></figcaption></figure>

### The EVE Frontier World on SUI

EVE Frontier World leverages SUI Move smart contracts for a scalable, high-performance, and on-chain EVE experience. The new architecture takes advantage of SUI's object-centric model, improved asset management, and transparent, trustless, and programmable interactions between game logic and player activity.

> Developers can find Eve Frontier World implementations and concrete integration examples within the [`evefrontier/world-contracts`](https://github.com/evefrontier/world-contracts) repository. The [`examples/`](https://github.com/evefrontier/world-contracts/tree/main/examples) directory contains tested scripts and patterns for interacting with the world, character, deployable, and storage unit systems.

Below is an overview of the key systems and their roles within the SUI-based EVE Frontier World.

***

### Systems Overview

#### SmartCharacter

**SmartCharacter** is a Move object representing a player's in-game persona and owned assets on the SUI blockchain.

* Each character is a unique, on-chain Move object that links to their inventory, skills, and progression.
* Ownership is mapped via SUI's native object ownership semantics; characters are non-transferrable (soulbound) for now, but future releases may enable trade/transfers using programmable object controls.
* Character creation and updates use Move entry functions (e.g., `create_character`, `update_profile`). See the Move module:\
  [`character.move`](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/character/character.move)

#### SmartAssembly

//TODO

#### SmartStorageUnit (SSU)

The **Smart Storage Unit (SSU)** is a managed, upgradeable, and uniquely owned storage facility for characters or groups.

* SSUs extend assembly logic, adding fine-grained inventory and access management.
* All SSU actions access controlled Move functions, enforcing in-game digital physics and on-chain resource safety.
* See:\
  [`smart_storage_unit.move`](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/assemblies/storage_unit.move)

**Inventory System**

//TODO: add new bridging concept from game to chain and chain to game and different access control mechanism to access main inventory, ephemeral inventory and extension mechanism to transfer between inventories

#### SmartGate System

The **SmartGate** system enables on-chain gates for world traversal and advanced movement mechanics in EVE Frontier.

Smart Gates are Move objects supporting these main features:

* **Anchor/Unanchor:** Deploy gates throughout the world, making them active ("anchored") or inactive ("unanchored").
* **Online/Offline:** Set gates online/offline, controlling access and operational state.
* **Link/Unlink:** Gates can be linked to each other, enabling networked travel between locations.
* **Jump:** Characters can execute a "jump" through a gate, which emits a `JumpEvent` with relevant context and participants.

**Security & Extension logic:**

* Jump mechanics support extension gating: if a gate is configured with a builder extension, jumps require presenting a single-use `JumpPermit` issued by extension logic. Otherwise, gates allow default jump access.
* This enables custom gameplay permissioning (e.g. ticketed/private gates, faction gating).

**Events:**

* Gate state changes and jumps emit events for tracking and external indexing.

**Example Operations:**

* Deploy (anchor) a gate in a solar system/location
* Link gates for player traversal routes
* Use extension logic to require JumpPermit tickets

***

### Integration Resources

* **Move Contract Repository & Examples:**
  * [evefrontier/world-contracts](https://github.com/evefrontier/world-contracts)
  * [`examples/`](https://github.com/evefrontier/world-contracts/tree/main/examples): Scripts and usage patterns for most major systems.

For further system details, consult the Move module docstrings and integration scripts within the `examples/` directory.

***
