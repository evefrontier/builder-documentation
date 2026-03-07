# Move Patterns in Frontier

Key Move patterns used in the EVE Frontier world contracts:

---

## Derived object ID

On-chain object IDs are **derived deterministically** from in-game identifiers (`item_id` + `tenant`) using Sui’s [derived objects](https://docs.sui.io/guides/developer/objects/derived-objects). A single `ObjectRegistry` ensures one in-game item maps to one on-chain object across characters, assemblies, and network nodes.

**Example:** IDs are claimed from the registry when creating assemblies (e.g. [assembly.move](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/assemblies/assembly.move)).  
**More:** [Object model](object-model.md) — items, `TenantItemId`, and derivation.

---

## Shared object model

Most world objects (characters, assemblies, network nodes) are **shared objects**. The game server (admin) and the object owner can both mutate the same object without transferring ownership. Access is enforced via capabilities, not object ownership.

**More:** [Object model](object-model.md).

---

## Hot potato

A **hot potato** is a struct with no `drop` ability. It must be consumed in the same transaction (e.g. by calling a function that takes it and destroys it). Frontier uses this to enforce multi-step flows.

**Examples in world contracts:**

- **ReturnOwnerCapReceipt** — Returned when borrowing an OwnerCap from a Character; it must be used to either return or transfer the cap, so the cap cannot be dropped or lost.

- **OfflineAssemblies** — [`network_node::offline`](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/network_node/network_node.move) returns a hot potato; the caller must bring all connected assemblies offline and then call `destroy_offline_assemblies` in the same transaction.

---

## Witness pattern (typed extension)

A **witness** is a one-time type that proves “this call comes from the module that defines this type.” Frontier uses it across **assemblies** so that only a specific extension module can authorize itself on an assembly. Once authorized, that custom contract can change the assembly's behaviour.

**Example:** [`gate::authorize_extension`](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/assemblies/gate.move) — the gate calls `type_name::with_defining_ids()` and stores that type as its extension. Only the module that defines that type could have produced the witness, so only that module’s logic can be registered. Other assemblies use the same pattern to allow custom contracts to extend their behaviour.

---

## Receiving as object (transfer to object)

OwnerCaps are **borrowed** from the Character object for a single transaction using Sui’s [transfer to object](https://docs.sui.io/guides/developer/objects/transfers/transfer-to-object) (e.g. `Receiving<OwnerCap<T>>`). The Character has a `Receiving` slot; the client passes a ticket to materialize the OwnerCap, uses it, then returns it (or transfers it) in the same transaction.

**In world contracts:** [character.move](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/character/character.move) — `borrow_owner_cap`, `return_owner_cap`.  
**More:** [Move book: Transfer to object](https://move-book.com/storage/transfer-to-object); [Ownership model](ownership-model.md) — borrow-use-return pattern.

---

## Dynamic fields

**Dynamic fields** let you attach key–value data to an object without changing the object’s type. Frontier uses them for ephemeral (e.g. temporary inventories) that can be added and removed at runtime.

**More:** [Move book: Dynamic fields](https://move-book.com/programmability/dynamic-fields).

---

## See also

- [Object model](object-model.md)
- [Ownership model](ownership-model.md)
- [EVE Frontier World Explainer](eve-frontier-world-explainer.md)
- [World contracts (GitHub)](https://github.com/evefrontier/world-contracts)
