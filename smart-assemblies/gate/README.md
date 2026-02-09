# Smart Gate

## Introduction

This guide explains the Smart Gate (interstellar jump gate) system for EVE Frontier on SUI. You’ll learn how gates are modeled as Move smart contract objects, how builders and players can deploy and manage gates, how to configure access with programmable logic, and how to test gate operations using the latest contracts and scripts from the [`evefrontier/world-contracts`](https://github.com/evefrontier/world-contracts) repository.

---

## Example Functionality

A Smart Gate in EVE Frontier is an on-chain object that enables permissioned travel between locations (solar systems, regions, etc). Gates can be configured with flexible access controls:

- Players or builders can anchor (deploy) gates and bring them online or offline.
- Gates can be linked to form travel networks, allowing jumps between distant locations.
- Access can be open, or restricted with custom logic—such as requiring a programmable JumpPermit for crossing (e.g., for faction/trust/guild gating, or fee-based travel).
- When a character jumps, a `JumpEvent` is emitted, recording the action for both gameplay and analytics.
- Builder extension patterns allow complex, composable travel conditions (e.g., integrations with other systems, external validation, or gameplay rule enforcement).

---

## Pre-Requisites

To work with Smart Gates:

- Follow the [Tools guide](../../Tools/) to set up your SUI development environment and all necessary dependencies.
- Deploy or connect to an EVE Frontier World instance as described in the [World Setup guide](../../LocalWorldSetup/).
- Familiarize yourself with Move modules and sample scripts in the [`evefrontier/world-contracts`](https://github.com/evefrontier/world-contracts) repo:
  - Review the `world/source/assemblies/gate` and `examples/gate/` directories for contract APIs and tested integration flows.

---

## Building and Interfacing with Smart Gate Contracts

Smart Gates are implemented as Move objects with public functions for setup, linking, configuring, and jump. Core operations include:

```move
public entry fun anchor_gate(
    caller: &signer,
    location_id: u64,
    ...
) {
    // Deploy and anchor a new Smart Gate at a given location
}

public entry fun bring_online(
    caller: &signer,
    gate: &mut SmartGate,
    ...
) {
    // Bring a gate online to allow travel
}

public entry fun link_gates(
    caller: &signer,
    gate_a: &mut SmartGate,
    gate_b: &SmartGate,
    ...
) {
    // Link two gates for networked jumps
}

public entry fun jump(
    character: &signer,
    gate: &mut SmartGate,
    maybe_permit: Option<JumpPermit>,
    ...
) {
    // Jump through a gate, requiring JumpPermit if extension logic is configured
}
```

- Gates optionally require a JumpPermit, a Move resource issued by builder extension logic, to enforce access control or programmable restrictions.
- All significant actions emit events (e.g., `JumpEvent`) for state auditing and off-chain analytics.

---

## Testing and Example Flows

- Use scripts in [`examples/gate/`](https://github.com/evefrontier/world-contracts/tree/main/examples/gate) to simulate anchoring gates, bringing them online/offline, linking, and performing travel with (or without) JumpPermits.
- Builder extension examples show how to issue JumpPermits based on custom rules, such as membership in a group or successful off-chain validation.
- Each successful gate action triggers event emissions; these can be monitored via SUI’s event system.

**Example test flow:**
1. Anchor and deploy a Smart Gate at a chosen location.
2. Bring the gate online.
3. Link it to another gate.
4. Use a character to jump through the gate (optionally presenting a JumpPermit for access).
5. Observe events to confirm successful travel and rule enforcement.

---

## Notes & Advanced Patterns

- Gates’ programmable access supports a wide range of gameplay and builder scenarios—from open road travel to exclusive/factional passage.
- Builder extension logic can be updated to reflect evolving gameplay, fees, or integrations.
- All gate and JumpPermit objects are first-class SUI objects and fully auditable via their object ID and associated events.

---

