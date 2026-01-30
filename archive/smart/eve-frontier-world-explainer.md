# EVE Frontier World Explainer

### The EVE Frontier World

The Eve Frontier World is a deployed [MUD World](/broken/pages/71a747aea2c13bb9acc8532b7e757311b27dae18), and an accompanying suite of EVE Frontier game related System contract for managing on-chain behavior, state, and functionality of game item representative [Smart Objects](/broken/pages/a53f4e14a569cc92ca75266d29a0729b8f82e129#classes-and-objects).

You can find an `npm` package of the EVE Frontier World [here](https://www.npmjs.com/package/@eveworld/world).

Below you will find a functionality and usage description for the primary EVE Frontier World Systems.

### Systems

#### SmartCharacterSystem.sol

Smart Characters are singleton Smart Objects which represent the on-chain functionality and state of an EVE Frontier game character.

Smart Character Objects are linked to a correlary ERC721 NFT token which shares the same ID as the Smart Object data structure. That is to say, the SOF `entityId` of a Smart Character (for tacking advanced functionality in MUD) and the ERC721 tokenId of the Smart Character (for tracking on-chain ownership) are the same ID.

Currently, Smart Character NFTs are **soulbound** (tokens bound to a single account which cannot be transferred to others). However, in the future as Character transfers become active, this restriction will be lifted.

You can find the `SmartCharacter` implementation [here](https://github.com/projectawakening/world-chain-contracts/blob/develop/mud-contracts/world/src/modules/smart-character/systems/SmartCharacter.sol).

#### SmartDeployableSystem.sol

Smart Assemblies are EVE Frontier's current primary on-chain to in-game interactable Objects. They have a manageable state `UNANCHORED`, `ANCHORED`, `ONLINE`, `DESTROYED` (which is primarily managed by the EVE Frontier game through game actions performed by the player).

However, there are a few points that should be noted for builders:

* when a Deployable is created, it also has an ERC721 NFT (with the same ID as the Deployable Object) minted to the player's account. Currently, similar to Smart Character, Deployable NFTs are [soulbound](/broken/pages/f82ed4e29d8c9c25f1957b8f639bbe921db5e067#smartcharactersol)
* when a Deployable is created its in-game ItemID and TypeID are stored on-chain in the [EntityRecord Module](https://github.com/projectawakening/world-chain-contracts/tree/develop/mud-contracts/world/src/modules/entity-record)
* when a Deployable becomes `ANCHORED` via game action, its fixed in-game location coordinates are stored on-chain in the [Location Module](https://github.com/projectawakening/world-chain-contracts/tree/develop/mud-contracts/world/src/modules/location)
* a Deployable cannot be set to `ONLINE` if it does not have enough FUEL to maintain its `ONLINE` status
* similarly a Deployable will be reverted back to an offline state (`ANCHORED` but not `ONLINE`) if it runs out of FUEL
* `UNNANCHORED` or `DESTROYED` Deployables will have any [Inventory](/broken/pages/f82ed4e29d8c9c25f1957b8f639bbe921db5e067#inventorysol) (or [EphemeralInventory](/broken/pages/f82ed4e29d8c9c25f1957b8f639bbe921db5e067#ephemeralinventorysol)) items expunged from the chain and passed back to the game. These are handled by the game logic for each case individually. `UNANCHORED` SSU inventory items will be returned to the player's ship inventory. `DESTORYED` SSU invnetory items (and any EphemeralInventory items) are jettisoned as wrekage and subject to in-game destruction rules.

Additionally, Smart Deployable owners do have access to two specific state changing functions:

* [bringOnline()](https://github.com/projectawakening/world-chain-contracts/blob/f114d550ff4116575644d665d0574ee57dbbeb18/mud-contracts/world/src/modules/smart-deployable/systems/SmartDeployable.sol#L127), a function for setting the Smart Deployable state to `ONLINE`, an active and usable state for interaction, and
* [bringOffline()](https://github.com/projectawakening/world-chain-contracts/blob/f114d550ff4116575644d665d0574ee57dbbeb18/mud-contracts/world/src/modules/smart-deployable/systems/SmartDeployable.sol#L144), a function for setting the Deployable state back to `ANCHORED`, making the Deployable inactive and inaccesible for interaction.

It should also be noted a Smart Deployable owner also has the permission to create [Hooks](/broken/pages/a53f4e14a569cc92ca75266d29a0729b8f82e129#hooks-and-hook-inheritance) that target both `bringOnline()` and `bringOffline()` in case they want to trigger specific logic for these state changes on their Deployable.

#### SmartStorageUnitSystem.sol

The Smart Storage Unit (SSU) is an extended-type of Smart Deployable. That is to say, SSUs have all of the Deployable functionality (ERC721 ownership, tracked state, Location, game data records, etc..) but additionally they also have an attached inventory for the SSU owner to deposit game items to and withdrawal game items from, as well as an attached game inventory window for non-owner interaction, which we call Ephemeral Inventory.

**InventorySystem.sol**

SSU Inventory exposes two functionalities which are interactable by the SSU owner:

* depositToInventory(), this function allows the SSU owner to stock their SSU with game items. Once game items are deposited to the SSU inventory, they are "burned" from the game state and are soley managed by the on-chain logic of the SSU
* withdrawalFromInventory(), this function enables the SSU owner to withdrawal items (either previously deposited items or new items deposited via other player interactions)
* Both of the above are game actions and managed through the [ERC2771 MetaTxn](/broken/pages/71a747aea2c13bb9acc8532b7e757311b27dae18#meta-transactions-metatxns) flow

The code for Inventory can be found [here](https://github.com/projectawakening/world-chain-contracts/blob/develop/mud-contracts/world/src/modules/inventory/systems/Inventory.sol).

**EphemeralInventorySystem.sol**

Ephemeral Inventory can be thought of as a player's game linked inventory window implemented on-chain which is used for interacting with a specific SSU of another player.

The owner of items can use the following functionality (in game) to place items into the Ephemeral Inventory window or remove items from it, thereby placing them on-chain for SSU interaction or returning them back to the player's in-game inventory:

* **depositToEphemeralInventory()**, allows a game item owner to place items into an on-chain inventory window linked to a specific SSU for interaction
* **withdrawFromEphemeralInventory()**, allows a game item owner to withdraw items from an on-chain SSU linked inventory back to their in-game inventory. Items in Ephemeral Inventory were either placed there previously by the item owner or were placed there from the linked SSU after an interaction
* Both of the above are considered game actions and are handled via the [MetaTxn](/broken/pages/71a747aea2c13bb9acc8532b7e757311b27dae18#meta-transactions-metatxns) flow

**InventoryInteractionSystem.sol**

This System exposes a transfer protocol between the SSU Inventory and any linked player Ephemeral Inventory.

The following functions expose this behavior for builder usage:

* **inventoryToEphemeralTransfer()**, allows a SSU owner to (or other approved accounts) transfer items from the SSU inventory to a chosen player's Ephemeral Inventory
* **ephemeralToInventoryTransfer()**, allows a game item owner to withdraw items from an on-chain SSU linked inventory window back to their in-game inventory. Items in Ephemeral Inventory were either placed there previously by the item owner or were placed there from the linked SSU after an interaction

SSU owners can configure access to the InventoryInteract functionality by setting APPROVED accounts on the SSU. This can be done with the following:

* **setApprovedAccessList()**, allows a SSU owner to set a list of accounts as an approved accounts for interacting with the **InventoryInteract** functionality for their SSU
* **setEphemeralToInventoryTransferAccess()**, allows an SSU owner to toggle APPROVED account access enforcement on/off for **Ephemeral Inventory transfers**
* **setInventoryToEphemeralTransferAccess()**, allows a SSU owner to toggle APPROVED account access enforcement on/off for **SSU Inventory transfers**

You can find a good behavioral description of usage for the above functionality in the [Smart Storage Unit](/broken/pages/e168f673a6e495b3446ec7044b08e26501f89588#ephemeral-inventory-to-ssu-inventory-interaction) section of the documentation.

**SmartGateSystem.sol**

Coming soon!

**SmartTurretSystem.sol**

Coming soon!

**KillMailSystem.sol**

Coming soon!
