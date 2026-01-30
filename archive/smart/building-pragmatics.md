# Building Pragmatics

## Frontend Frameworks

We intend building a DApp in EVE Frontier to be frontend-agnostic (meaning you can build with any frontend technologies you are comfortable with). That being said, we have provided a scaffold and example for builders to use and learn from which can be downloaded from: https://github.com/projectawakening/builder-examples/tree/develop/smart-assembly-scaffold

## Interacting with the OP Sepolia data Indexer

OP Sepolia is integrated with a MUD data indexer which reports the current state of any Table connected to the EVE Frontier World. You can view the MUD Table structure for EVE Frontier through the Table Definition File on GitHub: https://github.com/projectawakening/world-chain-contracts/blob/develop/mud-contracts/world-v2/mud.config.ts

The OP Sepolia namespace IDs will be published in that file as soon as the migration is complete.

## Interacting with game data

Every object on-chain has a unique uint256 identifier called a **Smart Object ID** including Smart Assemblies, Smart Characters, and items in storage. You can use this ID to find additional data through either the **World API** or **MUD Indexer**.

{% hint style="info" %}
A Smart Object ID is a uint256 identifier for on-chain objects (Smart Assemblies, Smart Characters, storage items). Use it to query the World API or the MUD Indexer for object state and metadata.
{% endhint %}

### World API

You can use the smart object's ID directly to lookup information about an Assembly or a Smart Character by calling, for example:

https://world-api-stillness.live.tech.evefrontier.com/v2/smartcharacters/0xcda43b6f62c3ccebdaf50afe2b9b1b46e196581a

This is useful for getting the current state of your Assembly without having to interact directly with the MUD data indexer and contract Table data. See the World API routes you can use here: /SwaggerWorldApi

### MUD Table Data

Once the public OP Sepolia MUD explorer is online you will be able to browse tables (such as `EntityRecord`) directly in your browser to find Type IDs and other metadata. Until then, rely on the World API responses described above or query the indexer programmatically when you have access credentials.

A typical request will look like:

{% code title="indexer-request.ts" %}
```typescript
const smartObjectId = '59026544381542288780477767110626007292283639148193460055157626650804930';
const indexerUrl = process.env.OP_SEPOLIA_INDEXER_URL ?? "https://graphql-stillness-internal.live.evefrontier.tech/v1/graphql";

const response = await fetch(indexerUrl, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify([
    {
      address: worldAddress.address,
      query: `SELECT "typeId" FROM evefrontier__EntityRecord WHERE "smartObjectId" = '${smartObjectId}';`,
    },
  ]),
}).then((res) => res.json());
```
{% endcode %}

## Chain Resources

You can find relevant information about the **OP Sepolia** chain configuration, deployed contract addresses, block explorer, and more here: /blockchains-stillness-list
