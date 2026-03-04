# Interfacing with the EVE Frontier World

## Overview

You interact with the EVE Frontier world in two ways:

- **Write path** — Submit transactions to Move public functions to mutate on-chain state (create assemblies, bring them online, deposit items, etc.).
- **Read path** — Query on-chain state and events via [GraphQL](https://docs.sui.io/guides/developer/accessing-data/query-with-graphql), [gRPC](https://docs.sui.io/guides/developer/accessing-data/grpc-overview), or custom indexers.

---

## Writing to the World Contracts

Write operations use the [Sui TypeScript SDK](https://sdk.mystenlabs.com/typescript) to build and submit transactions. The [world-contracts ts-scripts](https://github.com/evefrontier/world-contracts/tree/main/ts-scripts) provide examples on how to interact with the EVE Frontier world. You can also use other SDKs (e.g., [Rust](https://docs.sui.io/references/rust-sdk) or [community Go SDK](https://docs.sui.io/references/sui-sdks)) based on your tech stack. 

### Example: Bring Assembly Online

This script borrows `OwnerCap` from a character, calls the assembly `online` function, then returns the cap:

```typescript
import { Transaction } from "@mysten/sui/transactions";

// 1. Borrow OwnerCap from character
const [ownerCap] = tx.moveCall({
  target: `${config.packageId}::character::borrow_owner_cap`,
  typeArguments: [`${config.packageId}::assembly::Assembly`],
  arguments: [tx.object(characterId), tx.object(ownerCapId)],
});

// 2. Bring assembly online
tx.moveCall({
  target: `${config.packageId}::assembly::online`,
  arguments: [
    tx.object(assemblyId),
    tx.object(networkNodeId),
    tx.object(config.energyConfig),
    ownerCap,
  ],
});

// 3. Return OwnerCap to character
tx.moveCall({
  target: `${config.packageId}::character::return_owner_cap`,
  typeArguments: [`${config.packageId}::assembly::Assembly`],
  arguments: [tx.object(characterId), ownerCap],
});
```

See [ts-scripts/assembly/online.ts](https://github.com/evefrontier/world-contracts/blob/main/ts-scripts/assembly/online.ts) for the full script.

### Example: Sponsored Transactions

Many world operations require server-side validation (e.g., proximity checks). These use **sponsored transactions** — the player signs the intent, and an authorized sponsor (e.g., EVE Frontier) pays gas and submits:

```typescript
tx.setSender(playerAddress);
tx.setGasOwner(adminAddress);  // Sponsor pays gas

// ... moveCall to game_item_to_chain_inventory, etc.

const result = await executeSponsoredTransaction(
  tx, client, playerKeypair, adminKeypair,
  playerAddress, adminAddress,
  { showEvents: true }
);
```

See [ts-scripts/storage-unit/deposit-to-ephemeral-inventory.ts](https://github.com/evefrontier/world-contracts/blob/main/ts-scripts/storage-unit/deposit-to-ephemeral-inventory.ts) for a full example.



---

## Reading from the World Contracts

**SuiClient** is the main entry point for read operations in the [TypeScript SDK](https://sdk.mystenlabs.com/typescript). It connects to a Sui full node and exposes methods for querying objects, events, and transactions without submitting any transaction. Use it when you need to read state programmatically — for example, fetching an assembly's current config or checking ownership before building a transaction.

### GraphQL

Use Sui's [GraphQL RPC](https://docs.sui.io/guides/developer/accessing-data/query-with-graphql) to query objects by type, owner, or filters. 

**Example: Get objects by type**

```graphql
query GetObjectsByType($type: String!, $first: Int) {
  objects(filter: { type: $type }, first: $first) {
    pageInfo {
      hasNextPage
      endCursor
    }
    nodes {
      address
      asMoveObject {
        contents {
          json
        }
      }
    }
  }
}
```

Try it: [GraphQL Testnet IDE](https://graphql.testnet.sui.io/graphql). Pass variables in the IDE's Variables panel, e.g.:

```json
{
  "type": "0x2ff3e06b96eb830bdcffbc6cae9b8fe43f005c3b94cef05d9ec23057df16f107::network_node::NetworkNode",
  "first": 10
}
```

### gRPC

For higher throughput and streaming (e.g., checkpoints), use [gRPC](https://docs.sui.io/guides/developer/accessing-data/grpc-overview)—Sui's preferred read path. Requires a gRPC-enabled Sui full node.

```bash
# List objects owned by an address
grpcurl -d '{ "owner": "<Sui_address>" }' <full_node_url>:443 sui.rpc.v2.StateService/ListOwnedObjects
```

### Events

State changes emit events on transactions. Use [suix_queryEvents](https://docs.sui.io/guides/developer/accessing-data/using-events) to filter by module, type, or sender:

```bash
curl -X POST https://fullnode.mainnet.sui.io:443 \
  -H "Content-Type: application/json" \
  -d '{
    "jsonrpc": "2.0",
    "id": 1,
    "method": "suix_queryEvents",
    "params": [{
      "MoveEventType": "0x...::smartgate::JumpEvent"
    }, null, 10, false]
  }'
```

World events include `JumpEvent` (gate traversal), inventory updates, and deployment changes. Store and subscribe to events off-chain for dashboards, analytics, or game services.

---

## References

- [world-contracts ts-scripts](https://github.com/evefrontier/world-contracts/tree/main/ts-scripts) — TypeScript examples for world interactions
- [Sui GraphQL](https://docs.sui.io/guides/developer/accessing-data/query-with-graphql)
- [Sui gRPC](https://docs.sui.io/guides/developer/accessing-data/grpc-overview)
- [Sui Events](https://docs.sui.io/guides/developer/accessing-data/using-events)
- [EVE Frontier World Explainer](eve-frontier-world-explainer.md)
