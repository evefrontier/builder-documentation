# Smart Storage Unit (SSU) – EVE Frontier on SUI

## Introduction

This guide describes the Smart Storage Unit (SSU) system for EVE Frontier deployed on the SUI blockchain. It explains the core design of SSUs as Move smart contract objects, how to interact with them as a developer or player, and how to test SSU workflows using up-to-date contracts and practical examples from the [`evefrontier/world-contracts`](https://github.com/evefrontier/world-contracts) repository.

You'll learn how to set up your environment, interface with SSU storage and permissions, and use scripts to simulate real gameplay scenarios including atomic asset transfers, access control, and collaborative interactions.

---

## Example Functionality

A Smart Storage Unit (SSU) in EVE Frontier is an on-chain storage facility that supports secure and programmable item storage and transfer.

- Players can deposit and withdraw items from SSUs using permissioned Move entry functions.
- SSUs allow configuration of approved access lists and transfer rules so trusted parties or additional logic can interact or trade.
- Transfers between ephemeral (temporary, session-based) inventories and SSUs are supported for flexible trading and storage flows.
- Events are emitted for every successful action, allowing game services and external tools to monitor inventory state and transactions.
- Move functions validate permissions for all inventory actions, ensuring only owners or authorized accounts can access and use the storage.

---

## Pre-Requisites

Before working with SSUs:

- Follow the [Tools guide](../../Tools/) to set up your SUI development environment and required dependencies.
- Deploy or connect to an EVE Frontier World instance using the [World Setup guide](../../LocalWorldSetup/).
- Review the Move modules and example scripts in the [`evefrontier/world-contracts`](https://github.com/evefrontier/world-contracts) repo:
  - The `modules/storage/` and `examples/storage/` directories contain core contract logic and tested transaction flows.
  - Example scripts automatically hydrate object IDs from published world state, minimizing manual setup.

---

## Building and Interfacing with SSU Contracts

SSUs are implemented as Move objects with entry functions for storage operations, trading, and access management:

```move
public entry fun deposit_to_inventory(
    caller: &signer,
    ssu: &mut SmartStorageUnit,
    item_id: u64,
    amount: u64,
) {
    // Deposits items from player to SSU, emits an event, updates inventory state
}

public entry fun withdraw_from_inventory(
    caller: &signer,
    ssu: &mut SmartStorageUnit,
    item_id: u64,
    amount: u64,
) {
    // Withdraws items from SSU to player, emits event, checks permissions
}

public entry fun ephemeral_to_inventory_transfer(
    actor: &signer,
    ssu: &mut SmartStorageUnit,
    item_id: u64,
    amount: u64,
) {
    // Atomically moves items from ephemeral inventory to permanent SSU inventory
}
```

SSU owners can manage access lists and configure additional rules for authorized interactions with the contract APIs.

---

## Testing and Simulation

To test SSU interactions:

- Use scripts in [`examples/storage/`](https://github.com/evefrontier/world-contracts/tree/main/examples/storage) for automated flows covering deposits, withdrawals, transfers, and multi-user scenarios.
- Simulate access control by specifying approved account lists and executing transactions from different signers.
- Each successful transaction emits a SUI event, which can be tracked to verify game state and inventory activity.

A typical SSU workflow includes:
1. Deploy or initialize an SSU through Move or TypeScript scripts.
2. Deposit items to the SSU from your character/account.
3. Withdraw items from the SSU, observing permission logic and event outputs.
4. Transfer assets between ephemeral inventories and SSUs for trading or secure storage.

---

## Notes & Advanced Patterns

- SSU access flows are atomic, permissioned, and designed for future extension, supporting modular builder logic and advanced permissioning.
- Each SSU is a distinct SUI object, inspectable via SUI RPC or SDK, with all state and field data transparently available.
- Inventory events support integration with explorers, analytics, and custom game logic.

---
