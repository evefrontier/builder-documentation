# Development in the EVE Frontier World

The EVE Frontier smart contract environment is built around an Ethereum Virtual Machine (EVM) development workflow.

EVEnet is currently running on the [OP Sepolia testnet](https://community.optimism.io/docs/developers/networks/#optimism-sepolia-testnet), a modified Layer 2 chain.

This means builders should have a base level understanding of the Solidity programming language for development of any on-chain logic that interfaces with EVE Frontier's on-chain game logic.

## Solidity Version

Builders are required to use:

{% code title="Solidity pragma" %}
```solidity
pragma >=0.8.24;
```
{% endcode %}

You can find documentation for this version of Solidity [here](https://docs.soliditylang.org/en/v0.8.24/).
