# dApp Quick Start

The Eve DApp Scaffold is built with [Vite](https://vitejs.dev/), [Material UI](https://mui.com/material-ui/), [Tailwind](https://tailwindcss.com/), [React Router](https://reactrouter.com/en/main), and [Viem](https://viem.sh/). Before you begin:

{% stepper %}
{% step %}
#### Install pre-requisites

Follow the steps on [this page](/broken/pages/9a1ac951f89172bf1ed10e7c7b7c6f6c07af83c8) if you haven't already to install the required tools.
{% endstep %}

{% step %}
#### Setup local dev DApp

Run these commands to set up your local development DApp:

{% code title="Create and install" %}
```bash
npx @eveworld/create-eve-dapp my-dapp
cd my-dapp
pnpm install
```
{% endcode %}
{% endstep %}

{% step %}
#### Create a local `.env` by cloning the provided `.envsample`

Change the .env variables to be:

{% code title=".env" %}
```
```
{% endcode %}

```bash
VITE_SMARTASSEMBLY_ID=
VITE_GATEWAY_HTTP="https://blockchain-gateway-stillness.live.tech.evefrontier.com"
VITE_GATEWAY_WS="wss://blockchain-gateway-stillness.live.tech.evefrontier.com"
```

This gives the dApp an indexer endpoint to connect to, in this case for the world corresponding to the Stillness server.
{% endstep %}

{% step %}
#### Start DApp

Start the DApp by using:

{% code title="Start dev server" %}
```bash
pnpm dev
```
{% endcode %}
{% endstep %}

{% step %}
#### Open the DApp

Open http://localhost:3000/ to view your DApp.\
You should now be able to view the DApp in your browser.
{% endstep %}

{% step %}
#### Production

When you're ready to deploy to production, create a minified bundle with: (You can attach `--mode prod`)

{% code title="Build for production" %}
```bash
pnpm build:local
```
{% endcode %}
{% endstep %}
{% endstepper %}
