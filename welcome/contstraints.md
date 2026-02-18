# Builder Constraints and Limitations

While EVE Frontier is designed for unprecedented builder empowerment and programmability, there are important constraints and limitations—technical, design, and social—that all creators must understand when building for the live world.

## 1. Blockchain/Smart Contract Constraints

### - Resource and Gas Costs
- Every on-chain WRITE operation (deployment, transactions, storage) incurs gas costs. Reading state off-chain via gRPC or an indexer is free and efficient.
- Sui's resource-oriented model enforces that each change has a computation and storage price.

### - Execution & Storage Limits
- Smart Assemblies are subject to Sui’s per-transaction limits: transactions exceeding the maximum computation units abort, and transaction and object size are capped (Move objects max out at 250KB). Current limits are defined in [Sui protocol config](https://docs.sui.io/concepts/transactions#limits-on-transactions-objects-and-data).
- Move structs can have at most 32 fields.
- A maximum of 1024 dynamic fields can be accessed in a single transaction. See [Sui forums](https://forums.sui.io/t/how-does-the-size-limit-of-a-single-sui-move-object-work/45398) for details.
- Large datasets or high-frequency state changes are discouraged to avoid chain bloat.

### - Upgradeability Limits
- Published package code are immutable by default. Upgrades create new package versions on-chain (via UpgradeCap) and must respect layout compatibility; bug fixes or breaking changes may require data migration patterns, which should be planned in advance. refer https://docs.sui.io/guides/developer/packages/upgrade#upgrade-requirements for more details

### - Programming in Move
- All on-chain logic must be written in Move, which differs from previously supported EVM languages like Solidity.
- Move's strict type and resource system means some design patterns from other blockchains may not directly transfer.

---

## 2. Gameplay and World Constraints

### - Game Physics and Location
- Assemblies and assets are anchored to specific in-game locations and are bound by EVE Frontier’s digital physics. They cannot act outside physical limitations (e.g., instant transport, infinite reach).

### - Permissioning and Access
- Actions taken by Smart Assemblies must respect game-enforced permissions (character ownership, access rights, proximity, etc.).
- Some global or admin-level systems are still closed to player modification until future expansions.

### - Interoperability Boundaries
- Smart Assemblies interact with well-documented APIs, but there may be system features not yet exposed to builder code.
- Some world-changing actions (region creation, global rule changes) may be reserved for core updates or require approval/community consensus.

---

## 3. Documents and Community 

### - Documentation and Support
- Some APIs, features, or event triggers may not have full documentation; “bleeding edge” builders should expect evolving best practices.

### - Community Etiquette & Rules
- All builder contributions must follow the EVE Frontier code of conduct and collaboration guidelines.
- Actions intended to harass or grief others via programmable infrastructure are prohibited.

---

## 4. Future Expansion & Limitations

Many of these constraints will change as EVE Frontier and the Sui ecosystem—evolve. Builders are encouraged to:
- Design for upgradeability and modularity.
- Follow official changelogs and roadmaps for upcoming features or lifted restrictions.
- Share feedback so high-impact pain points can be prioritized.

--

Staying informed of current constraints lets you build robust, future-ready systems in EVE Frontier and help shape the evolution of the world!

