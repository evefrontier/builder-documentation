# Sui & Move Fundamentals

EVE Frontier runs on the [Sui](https://sui.io/) blockchain, leveraging the power and security of the [Move](https://docs.sui.io/concepts/sui-move-concepts) programming language. Understanding Sui’s architecture and Move’s unique features will unlock the full potential of developing Smart Assemblies and interactive systems for players.

---

## What is Sui?

**Sui** is a high-performance, Layer 1 blockchain designed for secure, scalable, and low-latency digital asset ownership and transfer. It is optimized for real-time, dynamic applications — like games and social platforms — where transaction throughput and parallelism matter.

**Key Sui Features:**
- **Low Latency:** Extremely fast transaction finality, suitable for real-time interactions.
- **High Throughput:** Scales horizontally by processing transactions in parallel, not just one at a time.
- **Object-Centric Model:** Accounts own unique objects, not just balances.
- **Native Support for Games:** Built with composability and upgradability in mind.

Learn more: [Sui Documentation](https://docs.sui.io/concepts)

---

## What is Move?

**Move** is a smart contract language originally built for blockchain applications and now integral to the Sui ecosystem. Move prioritizes security, resource management, and flexibility, making it ideal for complex interactions in games.

**Key Move Features:**
- **Resource-Oriented Programming:** Assets are treated as first-class resources. They can’t be duplicated or accidentally destroyed, preventing common bugs in digital asset management.
- **Modules and Scripts:** Logic is organized into reusable modules, while scripts interact with those modules to perform actions.
- **Strong Typing:** Move enforces strict type and ownership checks at compile-time and runtime.
- **Upgradeable and Composable:** Contracts can be designed for modular upgrades and extensibility.

Explore: [Move by Example](https://docs.sui.io/guides/developer/getting-started)

---

## How Frontier and Sui Work Together

- **Assets-as-Objects:** On Sui, every in-game item or assembly can be represented as a unique Sui object, tracked and manipulated with Move programs.
- **Ownership & Access Control:** Move enforces secure access, ensuring players control only the assets and assemblies they own.
- **Game Logic on-Chain:** Smart Assembly functions (storage, trading, combat, etc.) are securely programmed with Move, enabling safe and customizable gameplay.

**Example Use Cases:**
- Programmable structures and base-building with resource ownership.
- Player-driven trading systems and marketplaces.
- Secure and extendable on-chain game logic for collaborative or competitive mechanics.

---

## Getting Started

- [Learn Sui basics](https://docs.sui.io/concepts)
- [Try Move tutorials](https://docs.sui.io/guides/developer/getting-started/)
- [Explore Sui Dev Tools](https://docs.sui.io/references/sui-sdks)
