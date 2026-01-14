# deploy smart gate contracts

Firstly, you need to deploy the **Smart Gate** contracts to the world for them to work.

When deploying contracts, there are differences between using a local world on your machine and **Stillness (Live Server)**. Use the tabs below to select which world you are using to have the correct steps.

{% tabs %}
{% tab title="Local" %}
{% stepper %}
{% step %}
### Move to the example directory

Move to the contracts directory:

```bash
cd smart-gate
```
{% endstep %}

{% step %}
### Copy .envsample file

If you haven't already, copy the .envsample file to a .env file:

```bash
cp packages/contracts/.envsample packages/contracts/.env
```
{% endstep %}

{% step %}
### Install dependencies

Install the dependencies for the contracts:

```bash
pnpm install
```
{% endstep %}

{% step %}
### Fork + Deploy

Run a forked version of the local world / server and deploy the contracts:

```bash
pnpm dev
```

Once deployment is successful, you'll see a screen similar to the one below. This process creates a forked version of the local world and deploys the **Smart Gate** contracts.

The forked local world means that any changes that happen when running `pnpm dev` are reverted when closing it, allowing you to quickly reset and try something different.
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Stillness (Live Game)" %}
{% stepper %}
{% step %}
### Move to the Contracts Directory

```bash
cd smart-gate/packages/contracts
```
{% endstep %}

{% step %}
### Copy .envsample file

If you haven't already, copy the .envsample file to a .env file:

```bash
cp .envsample .env
```
{% endstep %}

{% step %}
### Configure Environment Variables

Set the following values in the new **.env** file to direct the scripts to use OP Sepolia:

```bash
WORLD_ADDRESS=0x1dacc0b64b7da0cc6e2b2fe1bd72f58ebd37363c
RPC_URL="https://op-sepolia-ext-sync-node-rpc.live.tech.evefrontier.com/"
CHAIN_ID=11155420
```

You can also automatically point to OP Sepolia with current values using:

```bash
pnpm env-op-sepolia
```
{% endstep %}

{% step %}
### Install dependencies

Install the dependencies for the contracts:

```bash
pnpm install
```
{% endstep %}

{% step %}
### Change private key

You need to change the private key and player private key variables to your private key. You can get this by going into your Wallet in-game.

Then, set the **PRIVATE\_KEY** in your .env file. Example file contents:

{% code title="smart-turret/packages/contracts/.env" %}
```
```
{% endcode %}

You can also use the below command and then input your private key to change it:

```bash
pnpm set-key
```
{% endstep %}

{% step %}
### Change namespace

A namespace is a unique identifier for deploying your smart contracts. Once you deploy to a namespace, it will set you as the owner and only you will be able to deploy smart contracts within the namespace.

Namespace rules:

* ✅ Use letters (a-z, A-Z)
* ✅ Use numbers (0-9)
* ✅ Use underscores (\_)
* ❌ No special characters
* ❌ No spaces

Change the namespace from **test** to your own custom namespace.

{% hint style="info" %}
Tip: Consider using your username or Tribe name as your namespace.
{% endhint %}

First, edit **packages/contracts/mud.config.ts** to include your new namespace:

{% code title="smart-turret/packages/contracts/mud.config.ts" %}
```
```
{% endcode %}

Then, edit **packages/contracts/src/systems/constants.sol**:

{% code title="smart-gate/packages/contracts/src/systems/constants.sol" %}
```
```
{% endcode %}

If you use an already existing Namespace that you do not own, it will not allow you to deploy the contracts and tables and will display the below error:

You can also use the below command and then input your new namespace to change it automatically:

```bash
pnpm set-namespace
```
{% endstep %}

{% step %}
### Deploy

Deploy the smart contract and tables using:

```bash
pnpm deploy:sepolia
```

Once deployment is successful, you'll see a screen similar to the one below. This process deploys the Smart Gate contract.
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}
