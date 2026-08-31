# Build a Custom Turret Extension

Step-by-step instructions for building a custom Turret extension that controls target priority. For concepts and behaviour, see the [Turret README](./README.md).

## Prerequisites

- Follow [environment-setup](../../quickstart/environment-setup.md)
- Complete the step-by-step instructions for the `builder-scaffold`: [builder-scaffold builder-flow](https://github.com/evefrontier/builder-scaffold/blob/main/docs/builder-flow.md)

## Extension Integration

For extension development and integration follow the [high-level build steps](../introduction#high-level-build-steps).

### Turret API overview

The [world turret module](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/assemblies/turret.move) exposes:

- **Lifecycle & ownership:** Owner-cap–guarded `online` / `offline`, metadata updates, and admin flows (anchor, unanchor, energy source). View functions: `status`, `location`, `is_online`, `owner_cap_id`, `energy_source_id`, `extension_type`, `is_extension_configured`, `is_extension_frozen`, etc.

- **Target priority:** The game calls `get_target_priority_list` whenever target behaviour changes (e.g. ship enters range, starts or stops attacking). Without an extension, the world module uses default rules. With an extension, the game resolves the package ID from the authorised type and calls **your** package’s `get_target_priority_list` with the same signature — you return a **BCS** ([Binary Canonical Serialization](https://sdk.mystenlabs.com/sui/bcs); Sui’s standard encoding format for on-chain data) encoded `vector<ReturnTargetPriorityList>` (target_item_id, priority_weight). The game shoots the highest-priority target; ties are resolved by list order.

- **Extension authorization:** Owner calls `authorize_extension<Auth>` to register a witness type. After that, only your extension’s `get_target_priority_list` is used. The world module’s `verify_online` returns an `OnlineReceipt` hot potato; your extension must consume it (e.g. `destroy_online_receipt<Auth>(receipt, auth)`).

- **Structs:** `TargetCandidate` (input from game), `ReturnTargetPriorityList` (output), `BehaviourChangeReason` (ENTERED, STARTED_ATTACK, STOPPED_ATTACK, etc.). Helpers: `unpack_candidate_list`, `unpack_return_priority_list`, `new_return_target_priority_list`.

## Smart Turret Extension API

Custom contracts use the **typed witness pattern**: define a witness struct (`Auth`) and register it on the turret. The [world turret module](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/assemblies/turret.move) verifies the type at runtime.

**Authorize an extension:**

```move
public fun authorize_extension<Auth: drop>(
    turret: &mut Turret,
    owner_cap: &OwnerCap<Turret>,
)
```

**Extension entry point (game calls this):** Expose a function with the **same signature** as the world turret module. The game deserialises `target_candidate_list` as a BCS encoded `vector<TargetCandidate>` and expects a BCS encoded `vector<ReturnTargetPriorityList>` back. Use `turret::unpack_candidate_list` and `turret::new_return_target_priority_list` / `turret::unpack_return_priority_list` as helpers.

If the function’s parameter types or return type do not match exactly, the game may not be 
able to read the values and the extension will not work correctly.

```move
public fun get_target_priority_list(
    turret: &Turret,
    owner_character: &Character,
    target_candidate_list: vector<u8>,
    receipt: OnlineReceipt,
): vector<u8>
```

**Verify turret is online (world module):**

```move
public fun verify_online(turret: &Turret): OnlineReceipt
```

The game obtains an `OnlineReceipt` before calling `get_target_priority_list`. The receipt is a hot potato and must be consumed: your extension should call `destroy_online_receipt<Auth>(receipt, auth)` before returning.

For ship group IDs and turret specialization by type, see the [Turret README](./README.md#ship-groups-and-turret-specialization).

---

<!-- TODO: When builder-scaffold has a full Turret extension example (like smart_gate), add sections here: 1. Understand the example contract, 2. Build and publish, 3. Authorize the extension on the turret, 4. Game integration / testing. For now use the Gate build guide as reference. -->

*Builder-scaffold example flow for Turret will be documented here once available. Until then, use the [Gate build guide](../gate/build.md) and [builder-scaffold](https://github.com/evefrontier/builder-scaffold) for publish and authorize patterns.*

## Reference

- [world-contracts](https://github.com/evefrontier/world-contracts) — EVE Frontier Sui Move contracts
- [turret.move](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/assemblies/turret.move) — core turret module (API, structs, helpers)
- [contracts/world](https://github.com/evefrontier/world-contracts/tree/main/contracts/world) — world contract package
