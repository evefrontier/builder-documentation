# Build a Custom Turret Extension

Step-by-step instructions for building a custom Turret extension that controls target priority. For concepts and behaviour, see the [Turret README](./README.md).

## Prerequisites

- Follow [environment-setup](../../quickstart/environment-setup.md)
- For the same typed-witness and publish flow (authorize extension, game calls into your package), see the [Gate build guide](../gate/build.md) and [builder-scaffold](https://github.com/evefrontier/builder-scaffold), use them as the source of truth for setup and build steps.

## High-level steps

1. **Define a witness struct** (e.g. `public struct TurretAuth has drop {}`) in your Move package.
2. **Implement `get_target_priority_list`** with the same signature as the [world turret module](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/assemblies/turret.move):
   - Parameters: `turret`, `owner_character`, BCS target list, `OnlineReceipt`
   - Return: BCS `vector<ReturnTargetPriorityList>`
   - Use `turret::unpack_candidate_list` and `turret::new_return_target_priority_list`; apply your priority rules (e.g. aggressors only, tribe/group filters, behaviour-based weights).
3. **Publish** the package and **authorize** it on the turret: borrow the turret's `OwnerCap` from the character and call `turret::authorize_extension<YourAuth>`.

After authorization, the game **resolves the package ID** from the authorised type and calls `get_target_priority_list` in your extension package whenever it evaluates targets for that turret.

## Turret API

Custom contracts use the **typed witness pattern**: define a witness struct (`Auth`) and register it on the turret.

**Authorize an extension:**

```move
public fun authorize_extension<Auth: drop>(
    turret: &mut Turret,
    owner_cap: &OwnerCap<Turret>,
)
```

**Extension entry point (game calls this):**  
Expose a function with the same signature as the [world turret module](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/assemblies/turret.move).

- The game deserialises `target_candidate_list` as BCS `vector<TargetCandidate>` and expects BCS `vector<ReturnTargetPriorityList>` back.
- Use `turret::unpack_candidate_list` and `turret::new_return_target_priority_list` / `turret::unpack_return_priority_list` as helpers.

If the function’s parameter types or return type do not match exactly, the game may not be able to read the values and the extension will not work correctly.

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

The game obtains an `OnlineReceipt` before calling `get_target_priority_list`. The receipt is a hot potato and must be consumed: extensions that call the world turret module should use `destroy_online_receipt<Auth>(receipt, auth)` before returning.

For ship group IDs and turret specialization by type, see the [Turret README](./README.md#ship-groups-and-turret-specialization).

## Reference

- [world-contracts](https://github.com/evefrontier/world-contracts) — EVE Frontier Sui Move contracts
- [turret.move](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/assemblies/turret.move) — core turret module (API, structs, helpers)
- [extension_examples/turret.move](https://github.com/evefrontier/world-contracts/blob/main/contracts/extension_examples/sources/turret.move) — example custom turret extension
- [contracts/world](https://github.com/evefrontier/world-contracts/tree/main/contracts/world) — world contract package
