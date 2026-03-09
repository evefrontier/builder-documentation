# Build a Custom Smart Gate

End-to-end build guide: write a gate extension, publish it, and test the complete jump flow. For concepts and behaviour, see the [Gate README](./README.md).

## Prerequisites

- Follow [environment-setup](../../quickstart/environment-setup.md)
- Complete step-by-step instructions: [builder-scaffold builder-flow](https://github.com/evefrontier/builder-scaffold/blob/main/docs/builder-flow.md)

## Gate API

Custom contracts use the **typed witness pattern**: define a witness struct (`Auth`) and register it on the gate. The [world gate module](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/assemblies/gate.move) verifies the type at runtime.

**Authorize an extension:**

```move
public fun authorize_extension<Auth: drop>(
    gate: &mut Gate,
    owner_cap: &OwnerCap<Gate>,
)
```

**Issue a jump permit (called by your extension):**

```move
public fun issue_jump_permit<Auth: drop>(
    source_gate: &Gate,
    destination_gate: &Gate,
    character: &Character,
    _: Auth,
    expires_at_timestamp_ms: u64,
    ctx: &mut TxContext,
)
```

**Jump with a permit:**

```move
public fun jump_with_permit(
    source_gate: &Gate,
    destination_gate: &Gate,
    character: &Character,
    jump_permit: JumpPermit,
    admin_acl: &AdminACL,
    clock: &Clock,
    ctx: &mut TxContext,
)
```

**JumpPermit** (from the world module): contains `character_id`, `route_hash`, and `expires_at_timestamp_ms`. The `route_hash` is direction-agnostic — a permit for Gate A → Gate B also works for Gate B → Gate A.

---

Below is the high-level understanding: 

## 1. Understand the example contract

The scaffold includes a working gate extension at `move-contracts/smart_gate/`. It has three modules:

**`config.move`** — shared configuration object and the witness type:

```move
public struct XAuth has drop {}

public struct ExtensionConfig has key { id: UID }

public struct AdminCap has key, store { id: UID }
```

`XAuth` is the witness type registered on the gate. `ExtensionConfig` is a shared object where extension modules store their rules as dynamic fields. `AdminCap` controls who can update those rules.

**`tribe_permit.move`** — issues jump permits only to characters in a configured tribe:

```move
public fun issue_jump_permit(
    extension_config: &ExtensionConfig,
    source_gate: &Gate,
    destination_gate: &Gate,
    character: &Character,
    clock: &Clock,
    ctx: &mut TxContext,
) {
    // Check character's tribe matches the configured tribe
    assert!(character.tribe() == tribe_cfg.tribe, ENotStarterTribe);

    // Issue a time-limited permit via the world gate module
    gate::issue_jump_permit<XAuth>(
        source_gate, destination_gate, character,
        config::x_auth(),
        expires_at_timestamp_ms,
        ctx,
    );
}
```

The key pattern: your module calls `gate::issue_jump_permit<XAuth>` with your witness type. The gate verifies `XAuth` matches the type registered via `authorize_extension`.

**`corpse_gate_bounty.move`** — a more advanced example that combines a storage unit interaction with a gate permit. Players deposit a bounty item and receive a jump permit in return.

## 2. Build and publish

```bash
cd move-contracts/smart_gate
sui client publish --build-env testnet
```

> For local network, provide the world package path:
> ```bash
> sui client publish --build-env testnet --pubfile-path ../../../world-contracts/contracts/world/Pub.localnet.toml
> ```

From the publish output, note the **package ID** and the **ExtensionConfig object ID**. Set both in your `.env`:

```
BUILDER_PACKAGE_ID=0x...
EXTENSION_CONFIG_ID=0x...
```

## 3. Configure extension rules

Set the tribe and expiry parameters on the shared `ExtensionConfig`:

```bash
pnpm configure-rules
```

This calls `tribe_permit::set_tribe_config` with a tribe ID and permit expiry duration. You can modify `ts-scripts/smart_gate/configure-rules.ts` to change the values.

## 4. Authorize the extension on both gates

Register your `XAuth` type on each gate so it switches from default (open) to permit-based:

```bash
pnpm authorise-gate
```

Under the hood this builds a transaction that:
1. Borrows the gate's `OwnerCap` from the character
2. Calls `gate::authorize_extension<XAuth>` on the gate
3. Returns the `OwnerCap` to the character

After this, the default `jump` function is disabled — players must use `jump_with_permit`.

## 5. Issue a jump permit

Issue a permit to a player character:

```bash
pnpm issue-tribe-jump-permit
```

This calls `tribe_permit::issue_jump_permit`, which checks the character's tribe and mints a `JumpPermit` object owned by the character.

Players can collect/buy permit from a builder dapp.

## 6. Jump with permit

The player uses their permit to jump through the gate:

```bash
pnpm jump-with-permit
```

A successful jump emits a `JumpEvent` and consumes the permit. Ideally this flow happens in the game.

## Writing your own extension

To create a gate extension from scratch:

1. Define a **witness struct** (e.g., `public struct MyAuth has drop {}`)
2. Write a function that enforces your rules and calls `gate::issue_jump_permit<MyAuth>`
3. Publish the contract and authorize it on your gate with `gate::authorize_extension<MyAuth>`

Here's a minimal toll gate that requires payment before issuing a permit:

```move
module custom::toll_gate;

public struct TollGateAuth has drop {}

public fun buy_pass<T>(
    source_gate: &Gate,
    destination_gate: &Gate,
    character: &Character,
    payment: Coin<T>,
    clock: &Clock,
    ctx: &mut TxContext,
) {
    // Custom logic: verify payment amount, token type, allowlist, etc.
    // ...

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

Other ideas:
- **Allowlist** — check a stored list of approved characters
- **Bounty gate** — require depositing an item to earn passage (see `corpse_gate_bounty.move` in the scaffold)

See the [Gate API](#gate-api) above for the full function signatures.

## Reference

- [world-contracts](https://github.com/evefrontier/world-contracts) — EVE Frontier Sui Move contracts
- [gate.move](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/assemblies/gate.move) — core gate module
- [builder-scaffold smart_gate](https://github.com/evefrontier/builder-scaffold/tree/main/move-contracts/smart_gate) — example gate extension
- [contracts/world](https://github.com/evefrontier/world-contracts/tree/main/contracts/world) — world contract package
