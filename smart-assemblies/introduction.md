<!-- TLDR version High-level overview of modding smart assemblies. Detailed info lives in dedicated pages. -->

# Introduction to Modding Smart Assemblies

Some EVE Frontier smart assemblies are **programmable** — you can customize their in-game behavior by deploying custom Move contracts.

<!-- TODO: Add a high level builder journey, eg: if you are builder interested in building smart contracts check out .. 
if you are somone interested in building tools check out ..
If you are somone who builds dapps check out this 
if you are philospoher/great thinker interested in creating new ideas check out ...
  -->

## Getting Started

Prerequisites to customize a smart assembly:

1. **Create a Character** — your on-chain identity that owns all your assemblies. See [Smart Character](./smart-character.md).

2. **Build a Network Node** — anchor a network node at a Lagrange point. This is the power source for your base. See [Network Node](./network-node.md).

3. **Deposit Fuel & Go Online** — deposit fuel into the network node and bring it online to start generating energy.

4. **Anchor a Smart Assembly** — create a smart assembly (e.g., Storage Unit, Gate) in your base. It automatically connects to the network node for energy.

5. **Bring the Assembly Online** — the assembly reserves energy from the network node and becomes operational.


> For local development and testing, refer to [builder-scaffold](https://github.com/evefrontier/builder-scaffold) so you have everything you need to directly write custom logic for your smart assembly. 

> For a one-command automated setup, see the community tool [efctl](https://frontier.scetrov.live/links/efctl/) ([docs](../tools/efctl.md)).

## Programmable Assemblies

Each assembly type has functionality that can be customized via the [extension pattern](../smart-contracts/eve-frontier-world-explainer#layer-3-player-extensions-moddability). For more details on which functionality can be extended, see the individual assembly type sections below. Each section has a **concept overview** (how the assembly works and its API) and a **build guide** (step-by-step instructions to write, publish, and test a custom extension):

- [Smart Gate](./gate/README.md) — custom rules for space travel (e.g., toll gates, access lists) · [Build](./gate/build.md)
- [Smart Storage Unit](./storage-unit/README.md) — custom rules for item deposits and withdrawals (e.g., vending machines, trade hubs) · [Build](./storage-unit/build.md)
- [Smart Turret](./turret/README.md) — custom targeting logic · [Build](./turret/build.md)

## High-level Build Steps

Each build process for assembly extension follows this same high-level pattern: 

1. Define a witness struct (e.g. `public struct Auth has drop {}`) in a separate custom Move package (separate from the EVE Frontier world package).

2. Implement extension logic in that same separate Move package such that it calls the assembly’s exposed API.

3. Publish your extension package and [authorize its witness](../smart-contracts/eve-frontier-world-explainer#layer-3-player-extensions-moddability) on each assembly object you want your extension to interact with. You must be the assembly `OwnerCap` holder to authorize extensions by [borrowing it from your Character object](../smart-contracts/ownership-model#borrow-use-return-pattern).

4. After authorization, your extension’s logic can provide extended functionality by interacting with `public` and `Auth`-gated API on an assembly object; the world module checks and enforces the `Auth` type authorization.

To read and write world state from code (SDK, GraphQL, gRPC), see [Interfacing with the EVE Frontier World](../tools/interfacing-with-the-eve-frontier-world.md).

## Extension freeze (optional)

Assembly owners can **freeze** the extension configuration on their object. Once frozen, the extension cannot be changed — the assembly is permanently bound to that extension package. This is **irreversible** and gives users of the extension a guarantee that the operating logic will not be updated (e.g. no rug-pull). Freeze only after the extension is audited or tested and you are comfortable with this permanence. Supported on Storage Unit, Gate, and Turret; for the API and usage see the [Storage Unit build guide](storage-unit/build.md#smart-storage-unit-extension-api) (which documents `freeze_extension_config`). Implementation: [extension_freeze.move](https://github.com/evefrontier/world-contracts/blob/main/contracts/world/sources/assemblies/extension_freeze.move).