# configure smart turret

Now that the **Smart Turret** contracts are deployed, you need to configure it and can customize it to change its functionality.

## Contracts

Similarly to deploying, select which world type you are using in the tabs below to configure your **Smart Turret**.

{% tabs %}
{% tab title="Local" %}
Configure the smart turret for **Local** using the instructions below.

{% tabs %}
{% tab title="Main" %}
The main workflow for Local:

{% stepper %}
{% step %}
### Mock data for the existing world

To generate mock data for testing the Smart Turret logic, run:

{% code title="Terminal" %}
```
```
{% endcode %}

```bash
pnpm mock-data
```

This will create the on-chain turret, fuel it, bring it online, and create a test smart character.

{% hint style="info" %}
Make sure you are running the commands in the **shell** process as visible in the image below. If you aren't in the shell, click on the shell process and then click on the terminal view.&#x20;
{% endhint %}
{% endstep %}

{% step %}
### Configure Smart Turret

To set the Smart Turret (turret ID), run:

{% code title="Terminal" %}
```
```
{% endcode %}

```bash
pnpm configure
```
{% endstep %}

{% step %}
### Test Smart Turret (Optional)

To test the Smart Turret's InProximity functionality, run:

{% code title="Terminal" %}
```
```
{% endcode %}

```bash
pnpm execute
```
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Unit Test Workflow" %}
The unit test workflow for Local:

{% stepper %}
{% step %}
### Run unit tests

To test the smart turret, execute:

{% code title="Terminal" %}
```bash
pnpm test
```
{% endcode %}

{% hint style="info" %}
Make sure you are running the command in the **shell** process as visible in the image below. If you aren't in the shell, click on the shell process and then click on the terminal view.&#x20;
{% endhint %}

You should then see the tests pass:&#x20;

The tests exercise different components of the turret and act as the equivalent of the "mock-data", "configure-smart-turret", and "execute" scripts.

See **smart-turret/packages/contracts/test/SmartTurretTest.t.sol** for details on how the tests work.
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}
{% endtab %}

{% tab title="Stillness (Live Game)" %}
Configure the smart turret for **Stillness** using the steps below:

{% stepper %}
{% step %}
### Setup the environment variables

Set the environment variables in `smart-turret/packages/contracts/.env` to the turret ID.

{% code title="smart-turret/packages/contracts/.env" %}
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
RPC_URL="http://127.0.0.1:8545" #Forked Anvil Local RPC Url
CHAIN_ID=31337 #Local Chain ID

# SMART TURRET CONFIG
# Copy the ID from the in-game smart turret
SMART_TURRET_ID=93802639558548716650424657917970819218391952910195344047336300457542616800807

# ALLOWED TRIBE
# What tribe won't be targeted by the turret
ALLOWED_TRIBE_ID=5001

# TESTING CONFIG
# This is the private key for tests / testing related scripts.
TEST_PLAYER_PRIVATE_KEY=0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80
```
{% endcode %}

Then set which tribe is not targeted by the turret by changing the `ALLOWED_TRIBE_ID` to your tribe ID.
{% endstep %}

{% step %}
You can also set these values using:

{% code title="Terminal" %}
```bash
pnpm set-config
```
{% endcode %}
{% endstep %}

{% step %}
### Configure Smart Turret

To configure the Smart Turret to use your custom Smart Contract and to set the allowed Tribe ID, run:

{% code title="Terminal" %}
```bash
pnpm configure
```
{% endcode %}
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}

### Troubleshooting

<details>

<summary><strong>World Address Mismatch</strong></summary>

Double-check that the `WORLD_ADDRESS` is correctly updated in the `contracts/.env` file. Make sure you are deploying contracts to the correct world.

</details>

<details>

<summary><strong>Anvil Instance Conflicts</strong></summary>

Ensure there is only one running instance of Anvil. The active instance should be initiated via the `docker compose up -d` command. Multiple instances of Anvil may cause unexpected behavior or deployment errors.

</details>

<details>

<summary><strong>Smart Turret ID Mismatch</strong></summary>

Ensure you have the correct Smart Turret ID when using Stillness in the `contracts/.env` file.

</details>

### Still having issues?

If you are still having issues, visit the troubleshooting page for more general tips: https://docs.evefrontier.com/Troubleshooting
