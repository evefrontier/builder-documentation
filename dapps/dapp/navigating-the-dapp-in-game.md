# Navigating the dApp (In-Game)

Every Smart Assembly comes with a Smart Assembly Base DApp. This base DApp lets you configure properties of Smart Assemblies that you own or interact with, and view aspects of others' Smart Assemblies.

{% stepper %}
{% step %}
### Open the Smart Assembly DApp

* In the EVE Frontier game client, navigate to a Smart Assembly, such as a smart storage unit.
* Click **Configure**. The in-game browser opens, showing the configuration DApp page for the Smart Assembly you selected.

The DApp displays primitive, view-only information for every Smart Assembly. This includes publicly viewable information about the Smart Assembly's owner, location, and state. Also viewable is information about any [attached modules](navigating-the-dapp-in-game.md#modules).
{% endstep %}
{% endstepper %}

### DApp Actions

The owner of a Smart Assembly can use the Smart Assembly Base DApp to set a custom name, description, and URL. These changes are on-chain actions, but are submitted as [metatransactions](https://eips.ethereum.org/EIPS/eip-2771) so they have no GAS cost to the player whether executed from within the EVE Frontier game client or from an external browser (ex. Chrome, Firefox, etc.).

{% stepper %}
{% step %}
### Edit unit metadata

* Click the "Edit unit" button.
* Make changes accordingly under DApp URL, Unit name or Unit description.
* Click **Save**. This signs and sends a message for an on-chain EIP-2771 metatransaction.
{% endstep %}
{% endstepper %}

{% hint style="info" %}
You must be a unit's owner to set its URL, name or description.
{% endhint %}

### Custom DApps

Some units may have an associated external URL for a website built and maintained by the unit's owner. These associated URLs allow builders to create UIs from which they can configure smart hooks easily. Only unit owners may add or edit URLs.

Users can view these custom DApps in-game, but should be aware that these DApps are entirely independent of the CCP safe zone.

Any functions called by external DApps are sent as gas-consuming transactions. Users must have ETH in their wallet in order to successfully call any builder functions. Users are fauceted a small amount of ETH on character creation, but an OP Sepolia faucet is [available here](https://ghostchain.io/faucet/ethereum-sepolia/) if more is required.

{% hint style="warning" %}
External DApp function calls consume ETH for gas. Ensure you have sufficient ETH in your wallet to complete transactions.
{% endhint %}

### Modules

Smart Assemblies are tagged as a Smart Storage Unit, Smart Turret or Smart Gate. These tags are detected in the Smart Assembly Base DApp, and their respective installed modules are then rendered within the base DApp.

#### Inventory Module

**Smart Storage Units** are comprised of a Smart Assembly with an attached inventory module. An inventory module grants storage capacity and lets players store inventory items in a Smart Assembly.

To interact with on-chain inventory items, users must link them to the chain through the _Ephemeral Inventory_ holding area. This creates a temporary on-chain link between a player's in-game inventory and the smart storage unit.

Once inventory items are deposited to the _Ephemeral Inventory_ holding area, they are accessible for on-chain transactions and other interactions. Items in a player's inventory are viewable in the Inventory Module UI of the DApp.

#### Gate Link Module

**Smart Gates** are comprised of a Smart Assembly with an attached gate link module. The gate link UI shows available Smart Gates that are in range and can be linked for a stargate jump.

Smart Gates can be linked by using the **link** function, which calls an on-chain `eveworld__linkSmartGates` function to link two stargates.
