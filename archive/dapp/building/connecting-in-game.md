# Connecting In-Game

To bring your custom dApp in-game, EVE Frontier provides a wallet provider injection in the in-game browser.

The dApp Scaffold adheres to [EIP-6963 Multi Injected Provider Discovery Standard](https://eips.ethereum.org/EIPS/eip-6963), and detects the EVE Frontier browser in-game wallet which is announced as `EVE Frontier Wallet`.

Use the following steps to detect and connect to the EVE Frontier Wallet.

{% stepper %}
{% step %}
### Detect the EVE Frontier Wallet

Add a window event listener for the `eip6963:announceProvider` event and request providers on page load. The example keeps track of all available EIP-6963-compliant injected providers.

{% code title="@eveworld/contexts/WalletContext" %}
```tsx
import { EIP6963ProviderDetail, SupportedWallets } from "@eveworld/types";
      
...
      
// Keep track of all available EIP6963-compliant injected providers
const [providers, setProviders] = useState<EIP6963ProviderDetail[]>([]);

useEffect(() => {
  function onPageLoad() {
    const availableProviders: EIP6963ProviderDetail[] = [];

    window.addEventListener("eip6963:announceProvider", (event: any) => {
      availableProviders.push(event.detail);
    });
    setProviders(availableProviders);
    window.dispatchEvent(new Event("eip6963:requestProvider"));
  }
  onPageLoad();
}, []);
```
{% endcode %}
{% endstep %}

{% step %}
### Connect using the named provider

From the list of announced providers, call `handleConnect` using the corresponding named provider. Import the supported wallet enum from `@eveworld/types`.

```tsx
import { SupportedWallets } from "@eveworld/types";
      
...
      
handleConnect(SupportedWallets.FRONTIER)
```

This will allow your custom dApp to recognize and connect to EVE Frontier in-game.
{% endstep %}
{% endstepper %}
