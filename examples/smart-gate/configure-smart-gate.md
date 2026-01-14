# configure smart gate

Now that the **Smart Gate** contracts are deployed, you need to configure it to work and can customize it to change its functionality.

## Contracts

Similarly to deploying, select which world type you are using in the tabs below to configure your **Smart Gate**.

{% tabs %}
{% tab title="Local" %}
Below is the guide for **Local** configuration of the smart gate example.

{% tabs %}
{% tab title="Main" %}
The **Main** workflow for **Local** is:

{% stepper %}
{% step %}
### Step 1: Mock data for the existing world

To generate mock data for testing the Smart Gate logic, run the following command:

{% hint style="info" %}
Make sure you are running the commands in the **shell** process as visible in the image below. If you aren't in the shell, click on the shell process and then click on the terminal view.
{% endhint %}

{% code title="Terminal" %}
```
```
{% endcode %}

```bash
pnpm mock-data
```

This will create the on-chain gates, fuel them, bring them online, and create a test smart character.
{% endstep %}

{% step %}
### Step 2: Configure Smart Gate

To configure which **Smart Gates** will be used, run:

{% code title="Terminal" %}
```
```
{% endcode %}

```bash
pnpm configure
```
{% endstep %}

{% step %}
### Step 3: Link Gates

To link the source and destination gates use:

{% code title="Terminal" %}
```
```
{% endcode %}

```bash
pnpm link-gates
```
{% endstep %}

{% step %}
### Step 4: Test Smart Gate (Optional)

To test the **Smart Gate**, use the following command:

{% code title="Terminal" %}
```
```
{% endcode %}

```bash
pnpm can-jump
```
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Unit Test Workflow" %}
The **Unit Test** workflow for **Local** is:

{% stepper %}
{% step %}
### Step 1: Run unit tests

To test the Smart Storage Unit, execute the following command:

{% code title="Terminal" %}
```
```
{% endcode %}

```bash
pnpm test
```

You should then see the tests pass:
{% endstep %}

{% step %}
The tests cover different components of the Smart Gate and act as the:

* **"mock-data"** - Generate the Smart Characters, Smart Gates, Fuel Them and Online Them
* **"link-gates"** - Link the Smart Gates Together
* **"configure"** - Configure the Smart Gates + Smart Contract
* **"can-jump"** - Check if Smart Characters are allowed to use the Smart Gates

See **smart-gate/packages/contracts/test/SmartGateTest.t.sol** for how the tests work.
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}
{% endtab %}

{% tab title="Stillness (Live Game)" %}
Below is the guide for **Stillness** configuration of the smart gate example.

{% stepper %}
{% step %}
### Step 1: Setup the environment variables

#### Smart Gate ID's

For Stillness, the smart gate id is available once you have deployed a Smart Gate in the game.

1. Right click your Source Smart Gate and click Interact.
2. Copy the Smart Gate ID.
3. Set the **SOURCE\_GATE\_ID** value in **smart-gate/packages/contracts/.env** to the Smart Gate ID.

{% code title="smart-gate/packages/contracts/.env" %}
```bash
# This .env file is for demonstration purposes only.
#
# This should usually be excluded via .gitignore and the env vars attached to
# your deployment environment, but we're including this here for ease of local
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

# SMART GATE CONFIG (Copy this info from in game smart gate)
# Source Smart Gate ID
SOURCE_GATE_ID=34818344039668088032259299209624217066809194721387714788472158182502870248994
# Destination Smart Gate ID=67387866010353549996346280963079126762450299713900890730943797543376801696007

# Allowed tribe
ALLOWED_TRIBE_ID=3434306

# TESTING CONFIG
# This is the private key for tests / testing related scripts.
TEST_PLAYER_PRIVATE_KEY=0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80
```
{% endcode %}

4. Repeat steps 1-3 for the **DESTINATION\_GATE\_ID**.
{% endstep %}

{% step %}
#### Allowed Tribe ID

Now set the Tribe ID here. You can retrieve the Tribe ID by:

1. Retrieve your character address from searching your username here: https://world-api-stillness.live.tech.evefrontier.com/v2/smartcharacters
2. Use this link: https://world-api-stillness.live.tech.evefrontier.com/v2/smartcharacters/ADDRESS and replace **"ADDRESS"** with the address from the previous step.
3. Find the **"tribeId"** value which should be in the response:

{% code title="World API Smart Character Result" %}
```json
{
  "address": "0x9dcd62f5c02e7066a3154bc3ba029e85345a5ce9",
  "id": "27968150122480120904130498262405934486185445355744041492535994892832439518842",
  "tribeId": "98000002",
  "name": "CCP Red Dragon",
  ...
}
```
{% endcode %}

4. Set the **ALLOWED\_TRIBE\_ID** to the Tribe ID.

{% code title="smart-gate/packages/contracts/.env (snippet)" %}
```bash
# SMART GATE CONFIG (Copy this info from in game smart gate)
# Source Smart Gate ID
SOURCE_GATE_ID=34818344039668088032259299209624217066809194721387714788472158182502870248994
# Destination Smart Gate ID=67387866010353549996346280963079126762450299713900890730943797543376801696007

# Allowed Tribe
ALLOWED_TRIBE_ID=3434306
```
{% endcode %}
{% endstep %}

{% step %}
#### Script

You can also set the Smart Gate ID's and Tribe ID through the command line using:

{% code title="Terminal" %}
```bash
pnpm set-config
```
{% endcode %}
{% endstep %}

{% step %}
### Step 2: Configure Smart Gate

To configure which gates to use, run:

{% code title="Terminal" %}
```bash
pnpm configure
```
{% endcode %}
{% endstep %}

{% step %}
### Step 3: Link Gates

On Stillness / The Live Game to link the source and destination gates you need to link them with the in-game UI. You can do this by approaching the Smart Gate, interacting with it and then linking them in the behavior window.
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}

### Next Steps

If everything went well, you should now have a Smart Gate pair that only allows members of a Tribe through. If you have any issues, make sure to read through the Troubleshooting section below.

Next, you can try the other examples like the [Smart Turret](/broken/pages/0e3e78b51e9bdbf16291dd0a4d2c391c98f0bfbd) or [Smart Storage Unit](/broken/pages/8478cb70b28d0cda57427ccf06b1ea7bed847ef8) examples or develop this Smart Gate example further to do new things.

### Troubleshooting

<details>

<summary>World Address Mismatch</summary>

Double-check that the `WORLD_ADDRESS` is correctly updated in the `contracts/.env` file. Make sure you are deploying contracts to the correct world.

</details>

<details>

<summary>Anvil Instance Conflicts</summary>

Ensure there is only one running instance of Anvil. The active instance should be initiated via the `docker compose up -d` command. Multiple instances of Anvil may cause unexpected behavior or deployment errors.

</details>

<details>

<summary>Wrong Gate ID's</summary>

If you are using OP Sepolia make sure you have set the **SOURCE\_GATE\_ID** and **DESTINATION\_GATE\_ID** correctly.

</details>

<details>

<summary>Not Linked</summary>

Make sure you link the gates as seen in Step 3, as otherwise the Smart Gates will not work.

</details>

### Still having issues?

If you are still having issues, then visit [the troubleshooting page](https://docs.evefrontier.com/Troubleshooting) for more general troubleshooting tips.
