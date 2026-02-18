# @evefrontier/dapp-kit

> React SDK for building EVE Frontier dApps on the Sui blockchain

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

Set the assembly ID via environment variable or URL parameter:

```env
# .env
VITE_SMARTASSEMBLY_ID=0x123...
```

Or via URL: `https://yourdapp.com/?assemblyId=0x123...`

### 3. Use the Hooks

```tsx
import { useConnection, useSmartObject, useNotification } from "@evefrontier/dapp-kit";

function MyDapp() {
  // Wallet connection
  const { isConnected, walletAddress, handleConnect, handleDisconnect } = useConnection();
  
  // Assembly data
  const { assembly, character, loading, error, refetch } = useSmartObject();
  
  // Notifications
  const { notify } = useNotification();

  if (!isConnected) {
    return <button onClick={handleConnect}>Connect Wallet</button>;
  }

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;

  return (
    <div>
      <p>Wallet: {walletAddress}</p>
      <p>Assembly: {assembly?.name}</p>
      <p>Owner: {character?.name}</p>
      <button onClick={handleDisconnect}>Disconnect</button>
    </div>
  );
}
```

## Core Concepts

### Hooks

| Hook | Description |
|------|-------------|
| `useConnection()` | Wallet connection state and methods |
| `useSmartObject()` | Current assembly data with auto-polling |
| `useSponsoredTransaction()` | Execute gas-sponsored transactions (EVE Frontier) |
| `useNotification()` | Display user notifications |
| `dAppKit.signTransaction()` | Sign a transaction (from @mysten/dapp-kit-react) |
| `dAppKit.signAndExecuteTransaction()` | Sign and execute a transaction |

### Standard Transactions (via dAppKit)

For custom transactions that are **not** sponsored, use the `dAppKit` object which wraps Mysten's `@mysten/dapp-kit-react`:

```tsx
import { dAppKit } from "@evefrontier/dapp-kit";
import { Transaction } from "@mysten/sui/transactions";

function CustomTransactionButton() {
  const signAndExecute = dAppKit.signAndExecuteTransaction();

  const handleCustomTx = async () => {
    // Build your transaction
    const tx = new Transaction();
    tx.moveCall({
      target: "0xpackage::module::function",
      arguments: [tx.pure.string("hello")],
    });

    // Sign and execute
    const result = await signAndExecute.mutateAsync({
      transaction: tx,
    });

    console.log("Transaction digest:", result.digest);
  };

  return (
    <button onClick={handleCustomTx} disabled={signAndExecute.isPending}>
      {signAndExecute.isPending ? "Signing..." : "Execute Custom Tx"}
    </button>
  );
}
```

#### Sign Only (without execution)

```tsx
import { dAppKit } from "@evefrontier/dapp-kit";

function SignOnlyExample() {
  const signTx = dAppKit.signTransaction();

  const handleSign = async () => {
    const tx = new Transaction();
    // ... build transaction

    const { signature, bytes } = await signTx.mutateAsync({
      transaction: tx,
    });

    console.log("Signature:", signature);
    // Execute later or send to backend
  };

  return <button onClick={handleSign}>Sign Transaction</button>;
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

### Sponsored Transactions

Execute gas-free transactions:

```tsx
import { useSponsoredTransaction, SponsoredTransactionActions } from "@evefrontier/dapp-kit";

function BringOnlineButton() {
  const { mutateAsync: sendTx, isPending } = useSponsoredTransaction();

  const handleClick = async () => {
    const result = await sendTx({
      txAction: SponsoredTransactionActions.BRING_ONLINE,
      chain: "sui:testnet",
    });
    console.log("Transaction:", result.digest);
  };

  return (
    <button onClick={handleClick} disabled={isPending}>
      {isPending ? "Processing..." : "Bring Online"}
    </button>
  );
}
```

Available actions:
- `SponsoredTransactionActions.BRING_ONLINE`
- `SponsoredTransactionActions.BRING_OFFLINE`
- `SponsoredTransactionActions.EDIT_UNIT`
- `SponsoredTransactionActions.LINK_SMART_GATE`
- `SponsoredTransactionActions.UNLINK_SMART_GATE`

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

// Check ownership
if (isOwner(assembly, currentAccount?.address)) {
  // Show owner controls
}

// Format storage capacity
formatM3(BigInt("1000000000000000000")); // 1.0

// Format burn time
formatDuration(3665); // "01h 01m 05s"

// Get transaction URL
getTxUrl("sui:testnet", txHash); // Suiscan URL

// Fetch item metadata
const info = await getDatahubGameInfo(83463);
console.log(info.name, info.iconUrl);
```

## API Reference

### Exports

```typescript
// Provider
export { EveFrontierProvider } from "./providers";

// Hooks
export { useConnection, useSmartObject, useSponsoredTransaction, useNotification } from "./hooks";

// dAppKit (Mysten's dapp-kit-react wrapper)
export { dAppKit } from "./config";
// Usage: dAppKit.signTransaction(), dAppKit.signAndExecuteTransaction()

// GraphQL
export { 
  executeGraphQLQuery,
  getAssemblyWithOwner,
  getObjectWithJson,
  getObjectsByType,
  // ... more
} from "./graphql";

// Types
export { Assemblies, AssemblyType, State, /* ... */ } from "./types";

// Utilities
export { abbreviateAddress, isOwner, formatM3, /* ... */ } from "./utils";

// Wallet
export { 
  walletSupportsSponsoredTransaction,
  getSponsoredTransactionFeature,
  EVEFRONTIER_SPONSORED_TRANSACTION,
} from "./wallet";
```