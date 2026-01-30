# World API

## Data Source Aggregation

DApps typically fetch information from multiple sources: a GraphQL endpoint for the Indexer; directly on-chain; and the IPFS metadata gateway. We have aggregated these into a single endpoint: the World API, which we expose with a REST API.

A hosted instance of the World API is available at https://blockchain-gateway-stillness.live.tech.evefrontier.com/docs/index.html

The World API exposes both REST API and Websocket endpoints.

### JavaScript example

To use the REST API with a JavaScript Promise to fetch information from the World API:

{% code title="example.js" %}
```javascript
Promise.resolve(
    fetch(`https://blockchain-gateway-stillness.live.tech.evefrontier.com/smartassemblies/<SMARTASSEMBLYID>`)
)
    .then((res) => res.json())
    .then(x => ...)
```
{% endcode %}

***

## Smart Assembly Information

The World API provides a structured way to access details about smart assemblies in JSON format. You can query a complete list of smart assemblies or fetch detailed information about a specific deployable by its unique identifier.

Fetching a list of all smart assemblies

* Endpoint: `/smartassemblies`
* Method: GET
* Description: Returns a JSON-formatted list of all smart assemblies available. Useful for obtaining an overview of deployables without specifying individual IDs.

Retrieving detailed information for a specific smart assembly

* Endpoint: `/smartassemblies/<SMARTASSEMBLYID>`
* Method: GET
* Description: Append the deployable's unique ID to the endpoint path to access detailed information about that smart assembly. This returns a JSON object containing comprehensive details.

JSON object structure

The JSON object returned when querying detailed information for a specific smart assembly includes the following fields:

* `id`: The unique identifier of the smart assembly.
* `itemId`: The unique in-game identifier of the smart assembly.
* `typeId`: The identifier for the type of the smart assembly.
* `ownerID`: The identifier of the owner of the smart assembly.
* `ownerName`: The character name of the owner of the smart assembly.
* `chainId`: The identifier for the chain the smart assembly resides on.
* `name`: An owner-set custom name for the smart assembly if specified.
* `description`: An owner-set custom description of the smart assembly, if specified.
* `dappUrl`: An owner-set custom URL for the dApp associated with the smart assembly, if specified.
* `image`: An image URL representing the smart assembly.
* `isOnline`: A boolean indicating whether the deployable is online.
* `state`: The current state of the deployable.
* `stateId`: An enum describing the current state of the deployable.
* `solarSystemId`: The ID of the solar system where the deployable is located.
* `solarSystem`: More detailed information about the solar system where the deployable is located, including:
  * `solarSystemId`: The ID of the solar system.
  * `solarSystemName`: The name of the solar system.
  * `solarSystemNameId`: An identifier for the solar system name.
* `region`: The region within the solar system of the smart assembly's location.
* `locationX, locationY, locationZ`: The X, Y, and Z coordinates representing the precise location of the smart assembly.
* `floorPrice`: The current market price of the smart assembly.
* `fuel`: Information about the fuel status of the deployable, including:
  * `fuelAmount`: The current amount of fuel.
  * `fuelConsumptionPerMin`: The rate at which fuel is consumed per minute.
  * `fuelMaxCapacity`: The maximum fuel capacity.
  * `fuelUnitVolume`: The volume of one unit of fuel.
* `assemblyType`: The type of the smart assembly (SmartTurret, SmartGate or SmartStorageUnit).

Additional fields depending on `assemblyType`

* `inventory` (for Smart Storage Units): A list of items or resources currently stored within the persistent inventory module of the smart assembly.
* `ephemeralInventory` (for Smart Storage Units): A list of player-owned ephemeral inventories that are attached to this smart assembly.
* `proximity` (for Smart Turrets): Information about proximity and aggression:
  * `inProximity`: Indicates if there are objects or entities in proximity to the deployable (null in this case).
  * `aggression`: Indicates if there is any aggression detected in proximity (null in this case).
* `gateLink` (for Smart Gates): Information about the linkage status of the SmartGate, including:
  * `gatesInRange`: A list of gates within range of the SmartGate (null or empty array if no gates are detected in range).
  * `isLinked`: A boolean indicating whether the SmartGate is linked to another gate.
  * `destinationGate`: The unique identifier of the corresponding Smart Gate which the SmartGate is linked.

Usage

Make HTTP GET requests using the appropriate URL structure. For example, to retrieve detailed information about a specific smart assembly, send a request to:

`/smartassemblies/<SMARTASSEMBLYID>`

(replace `<SMARTASSEMBLYID>` with the actual ID)

***

## Smart Character Information

Retrieving detailed information for a specific smart character

* Endpoint: `/smartcharacters/<0xSMARTCHARACTERID>`
* Method: GET
* Description: Append the associated wallet address to the endpoint path to access detailed information about a particular smart character. This returns a JSON object containing comprehensive details about the specified character.

JSON object structure

The JSON object returned when querying detailed information for a specific smart character includes the following fields:

* `address`: The hashed address of the wallet being queried.
* `id`: The unique identifier of the smart character (if available).
* `name`: The default name of the character.
* `isSmartCharacter`: A boolean representing whether the queried address is attached to a smart character.
* `eveBalance`: The current $EVE balance of the wallet.
* `gasBalance`: The current GAS balance of the wallet.
* `image`: The image associated with the smart character, if available.
* `smartAssemblies`: A list of smart assemblies owned by this smart character.

Usage

To retrieve detailed information about a specific wallet address, send a HTTP GET request to:

`/smartcharacters/<0xSMARTCHARACTERID>`

(replace `<0xSMARTCHARACTERID>` with the actual wallet address)

***

### Type information

To find item type information use `/types/TYPENUMBER` — for instance, `/types/77917`. Examples:

\*\* EXTRACTOR CHAMBER \*\* https://blockchain-gateway.nursery.reitnorf.com/types/77518

\*\* COMMON ORE \*\* https://blockchain-gateway.nursery.reitnorf.com/types/77800

\*\* HELIUM-4 \*\* https://blockchain-gateway.nursery.reitnorf.com/types/73005

***

### Chain information

To find configuration and other information about EVE Frontier's blockchains use the `/config` endpoint. This returns:

* ChainId (ex. 4541000)
* Name of the Chain
* Native currency (as `nativeCurrency`, including decimal value, name and symbol)
* Address of core EVE Frontier on-chain contracts
* RPC URL endpoints (as `rpcUrls`)
* Block explorer URL (as `blockExplorerURL`)

***

### Websockets

Websocket feeds for a given wallet address / Smart Character and Smart Assembly ID are available at:

`/ws/<0xSMARTCHARACTERID>/<SMARTASSEMBLYID>`

This returns a JSON object with two items: `smartCharacter` and `smartAssembly`. The type information of each of these items is the same as described above.
