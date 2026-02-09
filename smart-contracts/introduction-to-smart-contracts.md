# Introduction to Smart Contracts in EVE Frontier

## What are Smart Contracts?

Smart contracts are programs executed directly on the blockchain, responsible for enforcing persistent rules, managing assets, and automating system functions. In EVE Frontier, smart contracts are written in the Move language and deployed to SUI, where they interact as first-class modules or objects within the universe.

---

## Technical Structure

### Objects and Modules

- **Move Objects:** Every asset—whether a character, storage unit, deployable, or gate—is represented as a Move object on SUI. These objects have unique IDs, owners, and custom fields. Ownership and permission changes are encoded and enforced at the contract level.
- **Modules:** Smart contracts are organized as Move modules. Each module defines entry functions (transactional, system calls), struct types, events, and error codes. Modules encapsulate the game logic for systems like inventory, storage, deployables, gates, and economic flows.

### Entry Functions

- Entry functions are how gameplay and builder actions are performed.  
- Each entry function can require references to signers, objects, or resources and will mutate or verify state as part of a transaction.
- Entry functions are restricted by permission checks, object ownership, and simulation context.

**Example:**
```move
public entry fun deposit_to_inventory(
    owner: &signer,
    ssu: &mut SmartStorageUnit,
    item_id: u64,
    amount: u64,
) {
  // Move asset ownership, trigger event, update storage fields
}
```

### Events

- Contracts emit typed events as part of transaction execution.
- Events are indexed and queryable, allowing external systems (explorers, dashboards, dApps) to track game state, player actions, asset flows, and audit trails.

**Example:**
```move
struct InventoryDepositEvent has copy, drop {
  owner_id: address,
  ssu_id: object::ID,
  item_id: u64,
  amount: u64,
}
```

---

## Key Patterns

- **Atomicity:** Each transaction is processed atomically and either fully succeeds or reverts. All object references and mutations are bounded to SUI’s permission and ownership model.
- **Permissioning:** Move contracts enforce access—only the correct owners, signers, or whitelisted accounts can call certain functions or mutate certain state.
- **Composability:** Modules and objects can reference each other for complex flows (e.g., ephemeral inventory transfers, gate jumps with permits, storage access lists), supporting modular and extensible gameplay.

---

## Typical Contract Workflows

- **Deployment:** Modules are published to SUI by an authorized admin or builder. Each object (e.g., a Smart Storage Unit) is created, initialized, and assigned an owner.
- **Mutation:** Players or builders invoke entry functions to modify game state—deposit assets, link gates, update access lists, trigger events, etc.
- **Query:** Both on-chain clients and off-chain systems inspect object fields, read event logs, and monitor asset/ownership state for gameplay or analytics.
- **Extension:** Builders can register extension logic (such as gate JumpPermit issuance or custom access rules) to layer new mechanics over core modules.

---

## Building with Move

- Contracts are developed using Move’s module and object model.
- Code is organized by game system: character, deployable, storage, gate, market, etc.
- Developers can inspect and test modules using SUI tooling, Move unit tests, and the integration examples provided in [`evefrontier/world-contracts`](https://github.com/evefrontier/world-contracts).
- Scripts in the examples directory show how to compose transactions using hydrated object IDs, test permission flows, and benchmark contract performance.

---

## Summary

Smart contracts in EVE Frontier are precise, modular, and enforce all persistent gameplay, economic, and asset rules. Technical users can leverage the Move module and object structure, event system, and permission model to create, extend, and audit all core gameplay flows.  
For further technical reference, see module documentation, integration examples, and on-chain event types in the world contracts repository.

---
