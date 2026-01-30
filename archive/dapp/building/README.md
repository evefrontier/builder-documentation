# Building

This section describes how to attach new hook logic to an existing Smart Storage Unit, and how this hook logic can be accessed from the dApp Scaffold.

## Building with the Inventory Module

As previously mentioned, Smart Storage Units are comprised of a Smart Assembly with an attached Inventory Module. By default, Inventory Modules do not have any associated function logic beyond `inventoryIn` and `inventoryOut`.

Builders can leverage the Inventory Module's storage capabilities to transform a Smart Storage Unit into a Smart Vending Machine. The process involves augmenting the Smart Storage Unit with custom logic tied to its Inventory Module. By implementing this feature, developers can facilitate transactions where inserting a specific quantity (X) of one item results in dispensing a predetermined quantity (Y) of another item, effectively creating a builder-designed smart vending machine.

For ease of use, a builder might also want to create a UI in order to interact with their vending machine. EVE Frontier provides two potential starting points: the Smart Assembly Base DApp (for most builders), and the DApp Scaffold (for advanced builders).

This section assumes that builders have successfully written, tested, and attached smart contract logic to their Smart Storage Unit.

## Add ABI and Typescript declaration

To interact with a new smart contract, first create a new directory `src/contract`, then add the ABI (application binary interface) for the new contract along with a new context layer so that the initialised contract is accessible from within the new dApp. The folder structure looks like this:

```
my-new-eve-dapp/
└─── src/
    └── contract/
        └── IVendingMachine.ts
        └── VendingMachineContext.tsx
```

The Smart Assembly Base DApp and DApp Scaffold both use [viem](https://viem.sh/) to interface with Ethereum. The structure of `IVendingMachine.ts` should follow that of Viem v2.

## Initialize a connection

The Smart Assembly Base DApp and DApp Scaffold are both built with React Context for state management. Connections are initialized in context providers, then exported as custom hooks.

Viem requires using a PublicClient and WalletClient to initialize a contract. These are accessible using the `useConnection` hook, which is imported from `@eve/contexts`.

{% tabs %}
{% tab title="tsx" %}
{% code title="src/contract/VendingMachineContext.tsx" %}
```tsx
import { ReactNode, createContext, useContext, useMemo } from "react";
import {
  Client,
  getContract,
  GetContractReturnType,
  PublicClient,
  WalletClient,
} from "viem";
import IVendingMachine from "./IVendingMachine";
import { useConnection } from "@eveworld/contexts";
    
interface VendingMachineContract {
  vendingMachineContract: GetContractReturnType<
    typeof IVendingMachine.abi,
    Client
  > | null;
}
    
const VendingMachineProvider = ({ children }: { children: ReactNode }) => {
  const { defaultNetwork, publicClient, walletClient } = useConnection();
    
  const vendingMachineContract = useMemo(
    () =>
      getContract({
        abi: IVendingMachineAbi.abi,
        address: defaultNetwork.worldAddress,
        client: {
          public: publicClient as PublicClient,
          wallet: walletClient as WalletClient,
        },
      }),
    [publicClient, walletClient]
  );
    
  return (
    <VendingMachineContext.Provider value={{ vendingMachineContract }}>
      {children}
    </VendingMachineContext.Provider>
  );
};
    
export const VendingMachineContext = createContext<VendingMachineContract>({
  vendingMachineContract: null,
});
    
export default VendingMachineProvider;
```
{% endcode %}
{% endtab %}

{% tab title="ABI" %}
{% code title="src/contract/IVendingMachine.ts" %}
```ts
const IVendingMachine = {
  address: "0x294A791E5CeEA238C03CE4c2C1b094559E9AF6f1",
  abi: [
    ...,
    {
      type: "function",
      name: "vendingmachine__vend",
      inputs: [
        {
          name: "typeId",
          type: "uint256",
          internalType: "uint256",
        },
      ],
      outputs: [],
      stateMutability: "nonpayable",
    }
  ],
} as const;

export default IVendingMachine;
```
{% endcode %}
{% endtab %}
{% endtabs %}

Then, wrap your DApp within the new context layer in `main.tsx` in order to make it accessible:

{% code title="my-new-eve-dapp/src/main.tsx" %}
```tsx
ReactDOM.createRoot(document.getElementById("root")!).render(
 <EveWorldProvider>
    <VendingMachineProvider>
        <ThemeProvider theme={darkTheme}>
            <RouterProvider router={router} />
        </ThemeProvider>
    </VendingMachineProvider>
  </EveWorldProvider>
);
...
```
{% endcode %}

## Calling contract functions

After initializing the hook connection, call `useContext` to access the connected contract in any component. We recommend that any transactions being called are first simulated using `publicClient.simulateContract` before being sent as an on-chain transaction.

{% code title="src/components/NewComponent.tsx" %}
```tsx
import { VendingMachineContext } from "../contract/VendingMachineContext";
import { useConnection } from "@eveworld/contexts";
    
export default function NewComponent() {
  const { vendingMachineContract } = useContext(VendingMachineContext);
  const { publicClient, walletClient } = useConnection();
    
  const handleSendTransaction = async () => {
    const getTransactionRequest = async () => {
      if (!walletClient?.account) return;
      
      return publicClient.simulateContract({
        ...vendingMachineContract,
        functionName: "vendingmachine__callFunction",
        args: [
          arg1,
          arg2,
        ],
        account: walletClient.account.address,
      });
    }
    
    try {
      const transactionReq = await getTransactionRequest();
      transactionReq && await handleSendTx(getTransactionRequest.request);
      return;
    } catch (e) {
      console.error(e);
    }
  }
    
 return (
  <div>
    ...
  </div>
  )
}
```
{% endcode %}

## Adding Routes

The Smart Assembly Base DApp and DApp Scaffold use React Router v6.22. To access your new component, follow these steps:

{% stepper %}
{% step %}
### Create a route in `createBrowserRouter`

Add a new route object within `createBrowserRouter` in `src/main.tsx`:

{% code title="main.tsx" %}
```tsx
import NewComponent from "./components/NewComponent.tsx";

...

const router = createBrowserRouter([
  {
    element: <App />,
      children: [
        ...,
          {
            path: "new-path",
            element: <NewComponent />,
          }
      ],
  },
]);
```
{% endcode %}
{% endstep %}

{% step %}
### Visit the new route

Open your browser to: http://localhost:3000/new-path

You should see your new component.
{% endstep %}
{% endstepper %}
