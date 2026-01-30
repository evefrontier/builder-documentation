# MUD Explainer

### The MUD Framework

Within the EVM execution environment, EVE Frontier leverages the [MUD smart contract framework](https://mud.dev/introduction) for managing its on-chain game logic. The MUD framework diverges from traditional Solidity development patterns in several ways.

The most important of these differences include:

* a single entry point construction called the [World contract](https://github.com/latticexyz/mud/blob/main/packages/world/src/World.sol) which stores transaction data context and provides a single point of access to all of the registered resources of that world,
* the abstraction of the storage layer into Tables (with underlying Store) which in many ways mimics the interface of a traditional database,
* resource addressing using World unique identifiers for resource identification rather than a smart contract address to locate functionality, and
* the use of a config file to define the structure and resources of the world, and from this config the auto-generation of interface ABIs for the World, the Systems, and Tables therein.

MUD employs these design differences to maximize composability and extension of an existing World as well as management of large smart contract codebases, all of which lend themselves to the construction of complex [Autonomous Worlds](https://0xparc.org/blog/autonomous-worlds).

### Resources: Systems, Tables, and Namespaces

There are three primary resources in the MUD framework. Resources can be thought of as primitives or base components of the framework. Resources are assigned a [ResourceId](https://github.com/latticexyz/mud/blob/main/packages/store/src/ResourceId.sol) value which is used to uniquely identify each resource within the World.

The three defined resources are:

* Systems
* Tables
* Namespaces

#### ResourceIds

ResourceIds are a [deterministically generated](https://github.com/latticexyz/mud/blob/3baa3fd86f5917471729ba6551f12c17cdca53e3/packages/world/src/WorldResourceId.sol#L29) `bytes32` [user-defined value types](https://docs.soliditylang.org/en/v0.8.24/types.html#user-defined-value-types), being defined in three parts:

* a resource type in the first 2 bytes ([RESOURCE\_SYSTEM, RESOURCE\_TABLE, or RESOURCE\_NAMESPACE](https://github.com/latticexyz/mud/blob/main/packages/world/src/worldResourceTypes.sol))
* a namespace in the next 14 bytes, e.g. `bytes14("my-namespace")`, and
* a name in the last 16 bytes, e.g. `bytes16("my-resource-name")`

#### Systems

In the MUD framework, Systems are a set of defined functionality. They contain and expose the logic of a smart contract system via callable functions (including logic to read and write to Tables) but they do not contain contract storage in and of themselves.

#### Tables

Tables are an auto-generated library wrapper around the lower level [MUD Store](https://github.com/latticexyz/mud/tree/main/packages/store) layer. Tables expose the Table getter and setter interface as well as the `key` and `field` schema definitions and the full Table return data `struct` (if there is more than one field).

#### Namespaces

Namespaces are used in two ways in the MUD framework:

* Namespaces are used as part of a resource's ResourceId (including Namespace ResourceIds themselves) to ensure each resource within a World has a uniquely assigned ID for interaction.
* additionally, Namespaces are used for access control for the resources contained within them

In this way, the first property ensures that two Systems (or Tables) deployed to a single MUD world will not have colliding IDs, even if they have the same name. It also helps ensure that functions within those Systems (and Tables) are allowed to have the same [function selector](https://docs.soliditylang.org/en/v0.8.24/abi-spec.html#function-selector) across namespaces but will still have a uniquely identifiable ID via the single entry point of the World contract.

The second property allows for low level built-in access control within the MUD framework, following these rules:

* the deployer of (and account who [registers](https://github.com/latticexyz/mud/blob/3baa3fd86f5917471729ba6551f12c17cdca53e3/packages/world/src/codegen/interfaces/IWorldRegistrationSystem.sol#L16)) a Namespace is the owner of that Namespace.
* [Namespace owners](https://github.com/latticexyz/mud/blob/main/packages/world/src/codegen/tables/NamespaceOwner.sol) have rights to [register Systems](https://github.com/latticexyz/mud/blob/3baa3fd86f5917471729ba6551f12c17cdca53e3/packages/world/src/codegen/interfaces/IWorldRegistrationSystem.sol#L22) and Tables for that Namespace (tables are auto-registered to the Namespace defined in the `mud.config.ts` file)
* Namespace owners can [grant/revoke access](https://github.com/latticexyz/mud/blob/main/packages/world/src/codegen/interfaces/IAccessManagementSystem.sol) to specific resources registered in a Namespace (access can be checked [here](https://github.com/latticexyz/mud/blob/main/packages/world/src/AccessControl.sol))
* Namespace owners can [transfer ownership](https://github.com/latticexyz/mud/blob/3baa3fd86f5917471729ba6551f12c17cdca53e3/packages/world/src/codegen/interfaces/IAccessManagementSystem.sol#L18) to another account
* Only granted accounts are allowed to make write actions to protected resources
* By default any System registered in a Namespace has write access to the Tables within that same Namespace
* By default Tables are publicly readable

{% hint style="info" %}
NOTE: System deployments (and re-deployments) to an existing world will use the diamond pattern for updating the logic of the System, but Tables are immutable and cannot be updated in this manner.
{% endhint %}

#### Example

MySystem.sol

{% code title="MySystem.sol" %}
```solidity
pragma >=0.8.24;

import { System } from "@latticexyz/world/src/System.sol";
import { RESOURCE_SYSTEM, RESOURCE_TABLE, RESOURCE_NAMESPACE } from "@latticexyz/world/src/worldResourceTypes.sol";
import { ResourceId } from "@latticexyz/store/src/ResourceId.sol";
import { WorldResourceIdLib } from "@latticexyz/world/src/WorldResourceId.sol";

contract MySystem is System {
  bytes14 constant MY_NAMESPACE_NAME = bytes14("my-namespace");
  bytes16 constant MY_SYSTEM_NAME = bytes16("my-system-name");
  bytes16 constant MY_TABLE_NAME = bytes16("my-table-name");
  
  ResourceId constant public MY_NAMESPACE_ID = WorldResourceIdLib.encode({ typeId: RESOURCE_NAMESPACE, namespace: MY_NAMESPACE_NAME, name: "" });
  ResourceId constant public MY_SYSTEM_ID = WorldResourceIdLib.encode({ typeId: RESOURCE_SYSTEM, namespace: MY_NAMESPACE_NAME, name: MY_SYSTEM_NAME });
  ResourceId constant public MY_TABLE_ID = WorldResourceIdLib.encode({ typeId: RESOURCE_TABLE, namespace: MY_NAMESPACE_NAME, name: MY_TABLE_NAME });
  
  function tableWrite(uint256 tableKey, string testValue) {
    MyTable.setTestValue(MY_TABLE_ID, tableKey, testValue);
  }
  
  function tableRead(uint256 tableKey) public returns (string memory) {
    return MyTable.getTestValue(MY_TABLE_ID, tableKey);
  }
}
```
{% endcode %}

mud.config.ts

{% code title="mud.config.ts" %}
```ts
import { defineWorld } from "@latticexyz/world";

export default defineWorld({
  namespaces: { 
    my-namespace: { // namespace ResourceId will use the first bytes14 of this value, it must be unique among Namespaces
      tables: {
        MyTable: { // Table ResourceId will use the first bytes16 of this value, it must be unique among Tables
          schema: {
            myTableKey: "uint256",
            testValue: "string",
          },
          key: ["myTableKey"]
        },
      },
    },
  },
});
```
{% endcode %}

{% hint style="info" %}
NOTE: Systems are assigned to a namespace by being placed within the `/systems` folder of their respective namespace folder. e.g., `src/namespaces/my-namespace/systems/MySystem.sol`.
{% endhint %}

### MUD World Interaction

The MUD framework provides four ways to interact with the World.

#### Using world.call()

[world.call()](https://github.com/latticexyz/mud/blob/16121e8673adc72389a130cef64d73516c31d38c/packages/world/src/World.sol#L340) allows an external caller to make a call directly to a System within the MUD world, provided they know the `ResourceId` of the System they want to interact with, and the function selector of the method they want to call within that System. They can encode and pass the function selector together with the parameter values as they normally would for the [calldata](https://ethereum.org/en/developers/docs/transactions/#the-data-field) of a usual smart contract transaction.

`world.call()` is also often used for internal calls between Systems of the World, as it is the most gas efficient way to do so and it is the ONLY way for internal interaction between Systems within the ROOT Namespace (ROOT is the Namespace that is reserved for the MUD core logic).

world.call() Example

NOTE: assumes:

* Both MySystem and OtherSystem have been registered in the World, and
* OtherSystem (the System being called) has [publicAccess=true](https://github.com/latticexyz/mud/blob/16121e8673adc72389a130cef64d73516c31d38c/packages/world/src/modules/init/implementations/WorldRegistrationSystem.sol#L121). `publicAccess` is configurable in the `mud.config.ts`. It is `true` by default, or
* MySystem contract address has been granted access by the Namespace owner of Other System's Namespace

{% code title="world.call() example" %}
```solidity
pragma solidity >=0.8.24;

import { System } from "@latticexyz/world/src/System.sol";
import { IWorldCall } from "@latticexyz/world/src/IWorldKernel.sol";

import { RESOURCE_SYSTEM } from "@latticexyz/world/src/worldResourceTypes.sol";
import { ResourceId } from "@latticexyz/store/src/ResourceId.sol";

contract MySystem is System {

  bytes14 constant OTHER_NAMESPACE = bytes14("other-namespace");
  bytes16 constant OTHER_SYSTEM_NAME = bytes16("other-system-nam");

  ResourceId constant public OTHER_SYSTEM_ID = WorldResourceIdLib.encode({ typeId: RESOURCE_SYSTEM, namespace: OTHER_NAMESPACE, name: OTHER_SYSTEM_NAME });

  bytes memory data = world().call(OTHER_SYSTEM_ID, abi.encodeCall(IOtherSystem.functionName, (arg1, arg2, arg3, ...)));
}
```
{% endcode %}

* if successful, `data` will be the return data (or `0x` if there is no return data from the called function)
* if reverted, `data` will the revert error data (or `0x` if there was not revert message or Custom error defined)
* `data` is decoded via [abi.decode()](https://docs.soliditylang.org/en/v0.8.24/cheatsheet.html#abi-encoding-and-decoding-functions)

#### Using world.callFrom()

`world.callFrom()` is generally reserved for the World deployer to use to set various delegation rules for World interaction. For example, accounts could elect to delegate other accounts to call the World on their behalf. However, Namespace owners can also [set delegation rules for their Namespace](https://github.com/latticexyz/mud/blob/16121e8673adc72389a130cef64d73516c31d38c/packages/world/src/modules/init/implementations/WorldRegistrationSystem.sol#L303).

In the case of the [EVE World](/broken/pages/f82ed4e29d8c9c25f1957b8f639bbe921db5e067#the-eve-world), we have set a delegation for the EVE World Systems and Tables Namespace, such that the entire Namespace allows for an [ERC2771 Trusted Forwarder](https://eips.ethereum.org/EIPS/eip-2771) contract to be the delegatee for any incoming call. In this way, the EVE World supports [ERC712 encoded](https://eips.ethereum.org/EIPS/eip-712) MetaTxns for game originating actions.

**Meta-Transactions (MetaTxns)**

MetaTxns can be boiled down into the following flow:

* the acting account (the player's account) crypto-signs a MetaTxn payload from their account for security and verifiability (essentially a signature is tantamount approval of the data being signed)
* a transaction is built from this signed payload and submitted by a EVE Frontier ADMIN account through a Trusted Forwarder contract.
* the MUD World is configured to only accept transactions from the Trusted Forwarder as a delegatee

This allows game based player transactions to be submitted by the game itself on the player's behalf in a secure manner, while at the same time allowing players to undertake this activity without having to pay for the associated [network transaction fees](https://ethereum.org/en/developers/docs/gas/).

#### Using world.fallback()

Generally speaking, in Solidity the [fallback()](https://docs.soliditylang.org/en/v0.8.24/contracts.html#fallback-function) function behaves in the following manner. When a smart contract transaction makes a call to a contract and it cannot find the function selector from the transaction's `calldata` defined anywhere in the contract being called, then the `fallback()` is triggered.

Developers can then define logic in the `fallback()` function to handle such cases. For the MUD World contract, the `fallback()` handler logic forwards the call through to `world.call()` by recreating the SystemId from the `calldata` information.

When you generate a project using the MUD workflow, an `IWorld.sol` interface file gets auto-generated for your project. Any System included in your `mud.config.ts` file will have its own interface included in that `IWorld` interface. By using the corresponding [ABI](https://docs.soliditylang.org/en/v0.8.24/abi-spec.html) of this generated `IWorld` interface when building transactions, you are invoking the `fallback()` entry flow by calling the World contract directly with System level function data.

Example:

{% code title="IWorld fallback example" %}
```solidity
pragma solidity >=0.8.24;

import { System } from "@latticexyz/world/src/System.sol";
import { IWorld } from "../../../codegen/world/IWorld.sol";

contract MySystem is System {
  bytes memory data = IWorld(_world()).my-namespace__functionName(arg1, arg2, arg3, ...);
}
```
{% endcode %}

#### Using World System Libraries

The EVE Frontier World makes extensive use of the MUD System Libraries. Other deployments can also do so by adding the following to their `mud.config.ts` file:

{% code title="mud.config.ts (generateSystemLibraries)" %}
```ts
import { defineWorld } from "@latticexyz/world";

export default defineWorld({
  codegen: {
    generateSystemLibraries: true,
  },
  namespaces: {
    ...
  }
});
```
{% endcode %}

This will create a library interface for the Systems defined in your `namespaces/my-namespace/systems/` folder. These Systems can now be interacted with as follows:

{% code title="Using generated System library" %}
```solidity
import { otherSystem } from "../codegen/systems/OtherSystemLib.sol";

contract MySystem is System {
  bytes memory data = otherSystem.functionName(arg1, arg2, arg3, ...);
}
```
{% endcode %}
