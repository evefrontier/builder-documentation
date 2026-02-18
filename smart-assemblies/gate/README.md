# Gate

<figure><img src="../../.gitbook/assets/Gate.png" alt=""><figcaption></figcaption></figure>

## Introduction

A Gate is a structure in space that enables travel between locations. Two gates link together to create a transport route. Gates are **programmable** the owner can deploy custom extension contracts to control who can jump.

### Default Behavior

By default (no extension configured), **anyone can jump** through the gate without restrictions:

### Custom Behavior (Extension)

When the owner configures an extension, the gate switches to a **permit-based** model:

1. The owner deploys a custom Move contract that defines jump rules
2. The owner registers the contract as the gate's extension using `authorize_extension`
3. Players must obtain a `JumpPermit` from the extension logic, ideally through the builder's dapp before jumping
4. The `jump_with_permit` function validates the permit and allows the jump

```move
public struct JumpPermit has key, store {
    id: UID,
    character_id: ID,
    route_hash: vector<u8>,
    expires_at_timestamp_ms: u64,
}
```

The `route_hash` is direction-agnostic a permit issued for Gate A → Gate B also works for Gate B → Gate A.

For a full working example, see the [Custom Smart Gate Example](https://github.com/evefrontier/builder-scaffold/tree/main/move-contracts/smart_gate).

## Linking Gates

Two gates must be **linked** before anyone can jump between them. Requirements:
- Both gates must be owned by the same character
- Gates must be at least 20km apart (verified with a server-signed distance proof)

## Builder Extension Pattern

The extension uses the **typed witness pattern**. Your custom contract defines a witness struct, and the gate verifies its type at runtime.

### 1. Authorize the Extension

Register your custom contract type on the gate:


```typescript
const authType = `${builderPackageId}::${extensionModule}::XAuth`;
const tx = new Transaction();

// 1. Borrow the gate's OwnerCap from the character

// 2. Authorize the extension on the gate
tx.moveCall({
  target: `${packageId}::gate::authorize_extension`,
  typeArguments: [authType],
  arguments: [tx.object(gateId), gateOwnerCap!],
});

// 3. Return the OwnerCap to the character

```

### 2. Issue Jump Permits

Your extension contract calls `issue_jump_permit` with its witness type. The gate verifies the witness matches the registered extension:


### 3. Jump with Permit

Players use the permit to jump from the game.

### Example: Toll Gate Extension

```move
module custom::toll_gate;

public struct TollGateAuth has drop {}

public fun buy_pass(
    source_gate: &Gate,
    destination_gate: &Gate,
    character: &Character,
    payment: Coin<SUI>,
    clock: &Clock,
    ctx: &mut TxContext,
) {
    // Custom logic: verify payment, check allowlist, etc.
    // ...

    // Issue a time-limited jump permit
    gate::issue_jump_permit(
        source_gate,
        destination_gate,
        character,
        TollGateAuth {},
        clock.timestamp_ms() + 3600_000, // 1 hour expiry
        ctx,
    );
}
```

## Energy & Lifecycle

Gates follow the same energy model as all assemblies:
- Must be connected to a [Network Node](../network-node.md) for energy
- Must be brought **online** (reserves energy) before they can be used
- Going **offline** releases the reserved energy

## Next Steps 
Build a custom smart gate and test end to end with the instructions [here](./build.md)

**Reference:**
- [`gate.move`](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/assemblies/gate.move) — core gate contract
- [`smart_gate` extension example](https://github.com/evefrontier/builder-scaffold/tree/main/move-contracts/smart_gate/sources) — builder scaffold example
- [`world contracts`](https://github.com/evefrontier/world-contracts/tree/main/contracts/world) — world contract source
