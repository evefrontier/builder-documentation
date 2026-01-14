# configure smart storage unit

Now that the **Smart Storage Unit** contracts are deployed, you need to configure it to work and can customize it to change its functionality.

## Contracts

Similarly to deploying, select which world type you are using in the tabs below to configure your Smart Storage Unit.

{% tabs %}
{% tab title="Local" %}
Configure the Smart Storage Unit for **Local** using the below steps:

**Setting Items (Optional)**

You can set the items you want to trade for by changing the .env variables. The item `in` is the item bought and the item `out` is the item sold.

Set these in **smart-storage-unit/packages/contracts/.env**. You can find the Type ID using the World API Types Route and use the **id** value: https://world-api-stillness.live.tech.evefrontier.com//v2/types?limit=1000

{% code title="World API Types Result" %}
```json
{
  "data": [
    {
      "id": 72244,
      "name": "Feral Data",
      "description": "",
      "mass": 0.100000001490116,
      "radius": 1,
      "volume": 0.100000001490116,
      "portionSize": 1,
      "groupName": "Rogue Drone Analysis Data",
      "groupId": 0,
      "categoryName": "Commodity",
      "categoryId": 17,
      "iconUrl": ""
    },
    ...
  ...
}
```
{% endcode %}

{% code title="smart-storage-unit/packages/contracts/.env (example)" %}
```bash
# .envsample
# ----------------------------------------
# This file serves as a template for environment variables.
# DO NOT commit changes made to this file.
#
# To use local environment variables:
# 1. Duplicate this file.
# 2. Rename the duplicate to .env.
# 3. Update the .env file with your local environment settings.
#
# Example:
# cp .envsample .env
#
# Add the .env file to your .gitignore to prevent accidental commits.

# Enable debug logs for MUD CLI
DEBUG=mud:*
#
#Deployer [Currently set to the anvil default private key]
PRIVATE_KEY=0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80
WORLD_ADDRESS=0x0165878a594ca255338adfa4d48449f69242eb8f #Local World Address

#Local RPC
RPC_URL="http://127.0.0.1:8545" #Local RPC URL
CHAIN_ID=31337 #Local Chain ID

#SMART STORAGE UNIT ID
# Copy this info from the in-game Smart Storage Unit
SSU_ID=263171114538654274438749619211973208698572458800881337194040309134791495280

# Retrieve the item type ID's using https://world-api-stillness.live.tech.evefrontier.com/v2/types and search for the item name.
ITEM_IN_TYPE_ID=12345
ITEM_OUT_TYPE_ID=77800

#RATIOS
IN_RATIO=5
OUT_RATIO=1

# How many items to input each execute
EXECUTE_QUANTITY=5

#TESTING CONFIG
# This is the private key for tests / the execute script.
TEST_PLAYER_PRIVATE_KEY=0x59c6995e998f97a5a0044966f0945389dc9e86dae88c7a8412f4603b6b78690d
```
{% endcode %}

{% hint style="warning" %}
This used to use the items **Smart Object ID** but it now uses the **Type ID** due to the tenant system.
{% endhint %}

**Setting Ratios (Optional)**

A ratio with the in being 1 and out being 2 means for every item a player puts into the SSU, they get two items from it.

You can alter this ratio how you want, but be careful not to accidentally give away your whole supply of items with the wrong ratio.

Set these in **smart-storage-unit/packages/contracts/.env** (see example above).

{% tabs %}
{% tab title="Main" %}
The **Main** workflow for **Local** is:

{% stepper %}
{% step %}
### Step 1: Setup the environment variables

Configure the .env as shown above (optional adjustments for SSU\_ID, ITEM\_IN\_TYPE\_ID, ITEM\_OUT\_TYPE\_ID, IN\_RATIO, OUT\_RATIO, EXECUTE\_QUANTITY).
{% endstep %}

{% step %}
### Step 2: Mock data for the existing world

To generate mock data for testing the Smart Storage Unit logic on the local world, run the following command. This creates and anchors the Smart Storage Unit, smart characters and items.

{% hint style="info" %}
Make sure you are running the commands in the **shell** process as visible in the image below. If you aren't in the shell, click on the shell process and then click on the terminal view.<br>
{% endhint %}

```bash
pnpm mock-data
```

This will create the on-chain Gates, fuel them, bring them online, and create a test smart character.
{% endstep %}

{% step %}
### Step 3: Configure Smart Storage Unit

To configure which items should be traded and the ratios to trade for, run:

```bash
pnpm configure
```

You can adjust the values for the SSU\_ID, in and out item ID's and the ratios in the .env file as needed, though they are optional to change for local development.
{% endstep %}

{% step %}
### Step 4: Test The Smart Storage Unit (Optional)

To test the Smart Storage Unit, execute:

```bash
pnpm execute
```
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Unit Test Workflow" %}
The **Unit Test** workflow for **Local** is:

{% stepper %}
{% step %}
### Step 1: Setup the environment variables

Ensure your .env is configured (see example above).
{% endstep %}

{% step %}
### Step 2: Run unit tests

To test the Smart Storage Unit, execute:

```bash
pnpm test
```

You should then see the tests pass:&#x20;

The tests exercise different components of the SSU and act as the equivalent of running "mock-data", "configure-ratio", and "execute" in an automated way.

See `smart-turret/packages/contracts/test/SmartTurretTest.t.sol` for how the tests work.
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}
{% endtab %}

{% tab title="Stillness (Live Game)" %}
Configure the Smart Storage Unit for **Stillness** using the below steps:

For Stillness, the SSU id is available once you have deployed a Smart Storage Unit in the game.

* Right click your SSU and click Interact.
* Copy the SSU ID.
* Set the **SSU\_ID** value in **smart-storage-unit/packages/contracts/.env** to the SSU ID.

{% code title="smart-storage-unit/packages/contracts/.env (example for Stillness)" %}
```bash
# This .env file is for demonstration purposes only.
#
# This should usually be excluded via .gitignore and the env vars attached to
# your deployment enviroment, but we're including this here for ease of local
# development. Please do not commit changes to this file!
#
# Enable debug logs for MUD CLI
DEBUG=mud:*
#
#Deployer [Currently set to the anvil default private key]
PRIVATE_KEY=0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80
WORLD_ADDRESS=0x8a791620dd6260079bf849dc5567adc3f2fdc318 #Local World Address

#Local RPC
RPC_URL=http://127.0.0.1:8545 #Local RPC URL
CHAIN_ID=31337 #Local Chain ID

#SMART STORAGE UNIT ID
#Copy this info from the in-game Smart Storage Unit
SSU_ID=17614304337475056394242299294383532840873792487945557467064313427436901763824

# Retrieve the smart item ID's using https://blockchain-gateway-nova.nursery.reitnorf.com/v2/types and search for the item name. 
#ITEM IN : COILGUN AMMO 1 (S)
ITEM_IN_ID=72303041834441799565597028082148290553073890313361053989246429514519533100781
#ITEM OUT : LENS 3X
ITEM_OUT_ID=112603025077760770783264636189502217226733230421932850697496331082050661822826

#RATIOS
IN_RATIO=5
OUT_RATIO=1

# How many items to input each execute
EXECUTE_QUANTITY=5

# TESTING CONFIG
# This is the private key for tests / the execute script.
TEST_PLAYER_PRIVATE_KEY=0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80
```
{% endcode %}

**Setting Items (Optional)**

You can set the items you want to trade for by changing the .env variables. The item in is the item bought and the item out is the item sold.

Set these in **smart-storage-unit/packages/contracts/.env**. You can find the Type ID using the World API Types Route and use the **id** value: https://world-api-stillness.live.tech.evefrontier.com//v2/types?limit=1000

{% code title="World API Types Result" %}
```json
{
  "data": [
    {
      "id": 72244,
      "name": "Feral Data",
      "description": "",
      "mass": 0.100000001490116,
      "radius": 1,
      "volume": 0.100000001490116,
      "portionSize": 1,
      "groupName": "Rogue Drone Analysis Data",
      "groupId": 0,
      "categoryName": "Commodity",
      "categoryId": 17,
      "iconUrl": ""
    },
    ...
  ...
}
```
{% endcode %}

(See the .env example earlier for how to set ITEM\_IN\_TYPE\_ID / ITEM\_OUT\_TYPE\_ID and ratios.)

#### Step: Configure Smart Storage Unit

To configure which items should be traded and the ratios to trade for, run:

```bash
pnpm configure
```

You can adjust the values for the SSU\_ID, in and out item ID's and the ratios in the .env file as needed, though they are optional.

#### Step: Test The Smart Storage Unit (Optional)

To test the Smart Storage Unit, execute the following command:

```bash
pnpm execute
```
{% endtab %}
{% endtabs %}

## Troubleshooting

If you encounter any issues, refer to the troubleshooting tips below.

<details>

<summary><strong>World Address Mismatch</strong></summary>

Double-check that the `WORLD_ADDRESS` is correctly updated in the `contracts/.env` file. Make sure you are deploying contracts to the correct world.

</details>

<details>

<summary><strong>Anvil Instance Conflicts</strong></summary>

Ensure there is only one running instance of Anvil. The active instance should be initiated via the `docker compose up -d` command. Multiple instances of Anvil may cause unexpected behavior or deployment errors.

</details>

<details>

<summary><strong>Trade Quantity Is Incorrect</strong></summary>

Ensure your input and output ratios have been correctly set in the `contracts/.env` file.

</details>

<details>

<summary><strong>Item Type ID's are incorrect</strong></summary>

Ensure your `ITEM_IN_TYPE_ID` and `ITEM_OUT_TYPE_ID` are correct in the `contracts/.env` file.

</details>

### Still having issues?

If you are still having issues, then visit the troubleshooting page for more general troubleshooting tips: https://docs.evefrontier.com/Troubleshooting
