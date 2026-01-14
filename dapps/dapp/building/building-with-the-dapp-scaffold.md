# Building with the dApp Scaffold

## DApp Scaffold

If you prefer to build an entirely new custom DApp from scratch, the EVE Frontier DApp Scaffold is a bare-bones framework that provides a basic out-of-the-box connection to the blockchain. The DApp Scaffold provides:

* Connection to [EVE Vault](/broken/pages/b4e50609063d5e0cb5a71050e6884092d5be8fc0)
* React and viem installed
* State management with React Context
* Basic calls to the World API

{% code title="Create scaffold" %}
```bash
npx @eveworld/create-eve-dapp --type scaffold my-dapp
cd my-dapp
pnpm install
```
{% endcode %}

## Folder Structure

After cloning the DApp Scaffold repo, its folder structure is:

```
my-dapp/
├─ public/
├─ src/
│  ├─ components/
│  │  └─ EntityView.tsx
│  ├─ App.css
│  ├─ App.tsx
│  ├─ main.tsx
│  └─ vite-env.d.ts
├─ .envsample
├─ .gitignore
├─ index.html
├─ package.json
├─ postcss.config.js
├─ README.md
├─ tailwind.config.js
├─ tsconfig.json
├─ tsconfig.node.json
└─ vite.config.ts
```

## Environment Files

Create an `.env` file with the contents of `.envsample`. To point the Smart Assembly Base to a determined smart assembly, change the value of `VITE_SMARTASSEMBLY_ID` in the `.env` file.

Define the value of `VITE_NETWORK` to either `testnet` or `localnet`.

{% stepper %}
{% step %}
### Create the .env file

Copy the sample `.envsample` into a new `.env` file in the project root.
{% endstep %}

{% step %}
### Set key environment variables

Add or update the following values in `.env`:

{% code title=".env example" %}
```bash
VITE_SMARTASSEMBLY_ID="<SMARTASSEMBLYID>"
VITE_GATEWAY_HTTP="https://blockchain-gateway-stillness.live.tech.evefrontier.com"
VITE_GATEWAY_WS="wss://blockchain-gateway-stillness.live.tech.evefrontier.com"
```
{% endcode %}

* `VITE_SMARTASSEMBLY_ID`: the ID of the smart assembly to point the Smart Assembly Base to.
* `VITE_NETWORK`: set to either `testnet` or `localnet`.
{% endstep %}
{% endstepper %}

If no determinate value for `VITE_SMARTASSEMBLY_ID` is provided, the Smart Assembly Base will use the URL's `?smartObjectId=` query parameter to determine which smart assembly to interact with.

{% hint style="info" %}
If you would like to develop against a locally running instance of the chain, we recommend using the builder examples instead of the scaffold.
{% endhint %}
