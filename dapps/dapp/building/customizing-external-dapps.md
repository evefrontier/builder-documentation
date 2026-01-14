# Customizing External dApps

{% stepper %}
{% step %}
### Create Project

In order to start building a custom DApp, run:

{% code title="Create project / Run dev" %}
```bash
npx @eveworld/create-eve-dapp my-dapp
OR
npx @eveworld/create-eve-dapp --type scaffold my-dapp

cd my-dapp
pnpm install
pnpm dev
```
{% endcode %}
{% endstep %}

{% step %}
### Setup Environment Files

Create a local `.env` by cloning the provided `.envsample` and defining the value of `VITE_GATEWAY_HTTP` and `VITE_GATEWAY_WS` to either `testnet` or `localnet`. This will instruct the dApp to connect to either the testnet or localnet RPC URL, respectively. This value must be defined in order to interact with the dApp.

To fix interaction of the DApp to a determined smart assembly, change the value of `VITE_SMARTASSEMBLY_ID` in the `.env` file.

{% code title=".env example" %}
```bash
VITE_SMARTASSEMBLY_ID="<SMARTASSEMBLYID>"
VITE_GATEWAY_HTTP="https://blockchain-gateway-stillness.live.tech.evefrontier.com"
VITE_GATEWAY_WS="wss://blockchain-gateway-stillness.live.tech.evefrontier.com"
```
{% endcode %}

{% hint style="info" %}
If no determinate value for `VITE_SMARTASSEMBLY_ID` is provided, the Smart Assembly Base will use the URL's `?smartObjectId=` query parameter to determine which Smart Assembly to interact with.
{% endhint %}
{% endstep %}
{% endstepper %}
