# Interfacing with the EVE Frontier World

### Overview

The EVE Frontier World on SUI exposes Move smart contracts and on-chain modules for the game’s core systems. These contracts enable developers and builders to create, manage, and query in-game objects—such as characters, deployables, storage units, gates, and more—directly and securely on-chain.

Developers interact with the world through two primary mechanisms:

- **Write Path:** Submitting transactions to Move entry functions to mutate on-chain state and drive gameplay.
- **Read Path:** Querying on-chain object state and events via SUI RPC endpoints or indexers.

The [`examples/`](https://github.com/evefrontier/world-contracts/tree/main/examples) directory provides TypeScript and Move scripts demonstrating integration with all major systems, including gate operations and builder extension-based workflows.

---

### Write Path: Move Entry Functions for System Actions

All game actions that alter state are expressed as public entry functions in the Move modules. Each object (character, deployable, storage unit, gate, etc.) is a unique Move object/resource. Interactions require referencing the related object(s) and holding appropriate permissions.

Some of the core operations and their integration patterns include:

- **SmartCharacter and SmartDeployable:**  
  State changes—such as character creation, bringing a deployable online, or managing inventory—are performed using Move entry functions that check object references and authorization.

- **SmartStorageUnit (SSU):**  
  Asset deposit, withdrawal, and transfer between SSU inventory and ephemeral inventory are managed by specific entry functions, ensuring only authorized users perform item operations.

- **SmartGate System:**  
  Gates can be anchored, brought online or offline, linked to other gates, and used for jumps.  
  Gate jumps support two modes: a default open-access jump and an extension-gated mode where the user must present a single-use JumpPermit issued by custom builder logic for permissioned traversal.  
  All gate operations, including linking and permit-validated jumps, are handled with atomic Move transactions.

**Example - Gate and Jump Operations:**
```move
public entry fun bring_online(caller: &signer, gate: &mut SmartGate, ...) { ... }

public entry fun link_gates(
    caller: &signer,
    source_gate: &mut SmartGate,
    target_gate: &SmartGate,
    ...
) { ... }

public entry fun jump(
    character: &signer,
    gate: &mut SmartGate,
    maybe_permit: Option<JumpPermit>,
    ...
) { ... }
```
_See full usage examples in the [`examples/gate/`](https://github.com/evefrontier/world-contracts/tree/main/examples/gate) and builder extension scripts._

All such entry functions are documented in the module sources regarding parameters, permissions, and operational details.

---

### Read Path: Querying On-Chain State and Events

All chains of state and interactions are publicly queryable using SUI RPC, SDK, or indexer tools:

- **Object Inspection:**  
  Retrieve a game object’s fields (e.g., a character, deployable, storage unit, or gate) by SUI object ID.

- **Event Querying:**  
  Modules emit events such as `JumpEvent` (for gate traversal), `KillmailCreatedEvent` (for PVP loss records), and updates to inventories or deployments.  
  Events can be filtered by type or participant, supporting analytics, dashboards, and gameplay histories.

- **Integration Patterns:**  
  The example TypeScript and Move scripts automatically hydrate world object IDs, perform real-time queries, and log key events after transactions.

**Example (Pseudocode):**
```typescript
// Fetching an object by SUI object ID
getObject({ objectId: '0xSOME_ID...' });

// Query for all "JumpEvent" entries
getEvents({ eventType: '0x...::smartgate::JumpEvent' });
```

---

### Utilities & Best Practices

- **Object IDs:**  
  Each game object is a unique SUI on-chain object, referenced by its SUI object ID and protected by SUI’s ownership model.

- **Permissions:**  
  Write calls enforce owner or role-based access; extension logic can further restrict actions, such as requiring a JumpPermit for certain gate operations.

- **Atomicity:**  
  All operations are atomic per transaction, referencing all required Move objects and arguments.

- **Integration Support:**  
  The [`examples/`](https://github.com/evefrontier/world-contracts/tree/main/examples) directory offers patterns for configuring, hydrating, and invoking both system-level and permissioned flows, with robust error handling and output.

#### Integration Reference

- **Move Modules:** [`modules/`](https://github.com/evefrontier/world-contracts/tree/main/modules)
- **Example Scripts:** [`examples/`](https://github.com/evefrontier/world-contracts/tree/main/examples)
- **Module Documentation:** Docstrings and specifications in each module outline entry function signatures and logic.

---

### Example: Permissioned Gate Jump

```move
// Example of a jump that may be extension-gated
public entry fun jump(
    character: &signer,
    gate: &mut SmartGate,
    maybe_permit: Option<JumpPermit>,
    ...
) {
    // Logic verifies JumpPermit if the gate extension requires it; emits JumpEvent
}
```
See the builder extension examples for setting up custom JumpPermit logic or permissioned jump flows.

---
