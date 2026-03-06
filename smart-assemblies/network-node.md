# Network Node

The Network Node is the **power source** for a player's base. It burns fuel to produce energy (measured in GJ) which is consumed by all connected smart assemblies.

## Overview

A player anchors a Network Node at a Lagrange point to establish a base. All smart assemblies built in this base connect to the network node to draw energy for their operations.

## Fuel System

Fuel is deposited into the network node and burned over time. Different fuel types have different **efficiencies** that affect how long they last.

### Fuel Efficiency

Each fuel type has a configurable efficiency (10–100%). The **actual consumption rate** depends on the burn rate and fuel efficiency:

```
actualConsumptionRate = burnRateInMs × (fuelEfficiency / 100)
```

| Burn Rate | Fuel Efficiency | Actual Duration |
|---|---|---|
| 3600s (1 hour) | 100% | 60 minutes |
| 3600s (1 hour) | 80% | 48 minutes |
| 3600s (1 hour) | 50% | 30 minutes |
| 7200s (2 hours) | 70% | 84 minutes |

In-game fuel efficiency is represented by fuel type:

| Fuel type   | Efficiency |
| ----------- | ---------- |
| EU-90 Fuel  | 90%        |
| SOF-80 Fuel | 80%        |
| EU-40 Fuel  | 40%        |
| D2 FUEL     | 15%        |

### Fuel Lifecycle

1. **Deposit** — add fuel to the network node
2. **Start burning** — when the network node goes online, fuel burning begins and produces energy
4. **Stop burning** — saves remaining elapsed time for the next cycle
5. **Depletion** — when fuel runs out, the network node goes offline and all connected assemblies are forced offline

## Energy System

When the network node is online and burning fuel, it produces a fixed amount of energy (GJ). Assemblies **reserve** energy when they come online and **release** it when they go offline.

Each assembly type requires a fixed amount of energy to operate (configured in `EnergyConfig`):

```move
public struct EnergyConfig has key {
    id: UID,
    assembly_energy: Table<u64, u64>, // assembly_type_id -> energy required (GJ)
}
```

### Example

| Step | Action | Available Energy |
|---|---|---|
| 1 | Network node online, producing 100 GJ | 100 GJ |
| 2 | Storage Unit A connects and goes online (requires 50 GJ) | 50 GJ |
| 3 | Storage Unit B connects and goes online (requires 50 GJ) | 0 GJ |
| 4 | Cannot online another assembly — no energy left | 0 GJ |
| 5 | Storage Unit A goes offline, releases 50 GJ | 50 GJ |
| 6 | Gate connects and goes online (requires 1 GJ) | 49 GJ |

If the network node runs out of fuel, it goes **offline** — all connected assemblies are automatically brought offline.

<!-- TODO: Add a diagram -->

## Assembly Connection

Assemblies connect to a network node when they are anchored. The network node powers all connected assemblies:

### Hot Potato Pattern

Critical operations use **hot potato structs** to enforce atomicity within a single transaction:

- **`OfflineAssemblies`** — returned when a network node goes offline. Every connected assembly must be offlined in the same transaction.
- **`UpdateEnergySources`** — returned when assemblies are connected. Each assembly's energy source must be updated in the same transaction.
- **`HandleOrphanedAssemblies`** — returned when a network node is unanchored. All connected assemblies must be handled.

**Reference:**
- [`network_node.move`](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/network_node/network_node.move)
- [`fuel.move`](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/primitives/fuel.move)
- [`energy.move`](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/primitives/energy.move)
