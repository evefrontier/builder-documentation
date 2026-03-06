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

Each assembly type has its own extension pattern. Each section has a **concept overview** (how the assembly works and its API) and a **build guide** (step-by-step instructions to write, publish, and test a custom extension):

- [Smart Gate](./gate/README.md) — custom rules for space travel (e.g., toll gates, access lists) · [Build](./gate/build.md)
- [Smart Storage Unit](./storage-unit/README.md) — custom rules for item deposits and withdrawals (e.g., vending machines, trade hubs) · [Build](./storage-unit/build.md)
- [Smart Turret](./turret/README.md) — custom targeting logic · [Build](./turret/build.md)

To read and write world state from code (SDK, GraphQL, gRPC), see [Interfacing with the EVE Frontier World](../tools/interfacing-with-the-eve-frontier-world.md).