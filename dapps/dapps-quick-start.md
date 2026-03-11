# Getting Started

EVE Frontier Builder Scaffold dapps are built with [Vite](https://vitejs.dev/), [Tailwind](https://tailwindcss.com/), [React Router](https://reactrouter.com/en/main), [Mysten Dapp Kit](https://sdk.mystenlabs.com/dapp-kit/) and [EVE Frontier Dapp Kit](https://sui-docs.evefrontier.com/). Before you begin:

### Step 1: Install pre-requisites 

```bash copy
# PNPM
curl -fsSL https://get.pnpm.io/install.sh | sh -

# Node v22
nvm use 22
```

Also ensure that you have [EVE Vault](../eve-vault/browser-extension.md) installed.

### Step 2: Setup local DApp
Run these commands to set up your local development DApp:
```bash copy
git clone https://github.com/evefrontier/builder-scaffold.git
cd builder-scaffold/dapps
pnpm install
```

### Step 3: Create a local `.env` by cloning the provided `.envsample`
Change the .env variables to be:
```bash copy
# OPTIONAL Sui Object ID
VITE_OBJECT_ID=

# World package id and graphql endpoint
VITE_EVE_WORLD_PACKAGE_ID="0xf115375112eab1dcc1bb4af81a37d47ca7e95c2eb990cefa1f12f82d689e9543"
VITE_SUI_GRAPHQL_ENDPOINT="https://graphql.testnet.sui.io/graphql"
```

### Step 4: Start DApp 
Start the DApp by using:
```bash copy
pnpm dev
```

### Step 5: Open http://localhost:5173/ to view your DApp.
You should now be able to view the DApp in your browser.

![Alt text](./images/dapp-starter-screen.png)

Click on "Connect wallet" to see your wallet connected to the dapp.


![Alt text](./images/dapp-starter-connected.png)

### Step 6: Production 
When you're ready to deploy to production, create a minified bundle.

```bash copy
pnpm build
```