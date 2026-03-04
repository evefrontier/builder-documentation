# @evefrontier/dapp-kit

> React SDK for building EVE Frontier dApps on the Sui blockchain

**Full API documentation (TypeDoc): [http://sui-docs.evefrontier.com/](http://sui-docs.evefrontier.com/)**

## Features

- 🔌 **Wallet Connection** - Easy integration with EVE Vault and Sui wallets
- 📦 **Smart Object Data** - Fetch and transform assembly data via GraphQL
- ⚡ **Sponsored Transactions** - Gas-free transactions via EVE Frontier backend
- 🔄 **Auto-Polling** - Real-time updates with automatic data refresh
- 🎨 **TypeScript First** - Full type safety for all assembly types

## Installation

```bash
npm install @evefrontier/dapp-kit
# or
pnpm add @evefrontier/dapp-kit
```

### Peer Dependencies

```bash
npm install @tanstack/react-query react
```

> Keep your app’s versions of **react**, **@mysten/dapp-kit-react**, and **@mysten/sui** in sync with the versions used by this package to avoid type or runtime mismatches. Check `package.json` for the current ranges.

## Quick Start

### 1. Set up the Provider

Wrap your app with `EveFrontierProvider`:

```tsx
import { EveFrontierProvider } from "@evefrontier/dapp-kit";
import { QueryClient } from "@tanstack/react-query";

const queryClient = new QueryClient();

function App() {
  return (
    <EveFrontierProvider queryClient={queryClient}>
      <MyDapp />
    </EveFrontierProvider>
  );
}
```

### 2. Configure Assembly ID

Set the assembly ID via URL parameter:

`https://yourdapp.com/?tenant=utopia&itemId=...`

### 3. Use the Hooks

```tsx
import { EveFrontierProvider, useConnection, useSmartObject } from "@evefrontier/dapp-kit";
import { QueryClient } from "@tanstack/react-query";

const queryClient = new QueryClient();

function App() {
  return (
    <EveFrontierProvider queryClient={queryClient}>
      <MyDapp />
    </EveFrontierProvider>
  );
}

function MyDapp() {
  const { isConnected, handleConnect } = useConnection();
  const { assembly, loading } = useSmartObject();
  if (!isConnected) return <button onClick={handleConnect}>Connect</button>;
  if (loading) return <div>Loading...</div>;
  return <div>{assembly?.name}</div>;
}
```

## Core Concepts

### Hooks

| Hook | Description |
|------|-------------|
| `useConnection()` | Wallet connection state and methods |
| `useSmartObject()` | Current assembly data with auto-polling |
| `useNotification()` | Display user notifications |
| `dAppKit.signTransaction()` | Sign a transaction (from @mysten/dapp-kit-react) |
| `dAppKit.signAndExecuteTransaction()` | Sign and execute a transaction |

### Standard Transactions (via dAppKit)

For normal transactions, use the `dAppKit` object which wraps Mysten's `@mysten/dapp-kit-react`:

```tsx
import { useDAppKit } from "@mysten/dapp-kit-react";
import { Transaction } from "@mysten/sui/transactions";

function CustomTransactionButton() {
  const dAppKit = useDAppKit();

  const handleCustomTx = async () => {
    // Build your transaction
    const tx = new Transaction();
    tx.moveCall({
      target: "0xpackage::module::function",
      arguments: [tx.pure.string("hello")],
    });

    // Sign and execute
    const result = await dAppKit.signAndExecute({
      transaction: tx,
    });

    console.log("Transaction digest:", result.digest);
  };

  return (
    <button onClick={handleCustomTx}>
      Execute Tx
    </button>
  );
}
```

### Assembly Types

The SDK supports all EVE Frontier assembly types:

```typescript
import { Assemblies, AssemblyType } from "@evefrontier/dapp-kit";

// Available types
Assemblies.SmartStorageUnit
Assemblies.SmartTurret
Assemblies.SmartGate
Assemblies.NetworkNode
Assemblies.Manufacturing
Assemblies.Refinery
```

## GraphQL API

For custom data fetching:

```typescript
import { 
  getAssemblyWithOwner,
  getObjectWithJson,
  executeGraphQLQuery 
} from "@evefrontier/dapp-kit";

// Fetch assembly with owner
const { moveObject, character } = await getAssemblyWithOwner("0x123...");

// Fetch any object
const result = await getObjectWithJson("0x456...");

// Custom query
const data = await executeGraphQLQuery(myQuery, { address: "0x789..." });
```

## Utilities

```typescript
import {
  abbreviateAddress,
  isOwner,
  formatM3,
  formatDuration,
  getTxUrl,
  getDatahubGameInfo,
} from "@evefrontier/dapp-kit";

// Shorten address for display
abbreviateAddress("0x1234567890abcdef"); // "0x123...cdef"

// Fetch item metadata
const info = await getDatahubGameInfo(83463);
console.log(info.name, info.iconUrl);
```

## API Reference

**Full API documentation (TypeDoc): [http://sui-docs.evefrontier.com/](http://sui-docs.evefrontier.com/)**