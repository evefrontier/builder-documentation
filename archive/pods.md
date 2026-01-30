# Introduction to PODs

Documentation around PODs is currently a work in progress.

## Target Audience

This documentation is written to be a beginner’s guide to **PODs** and **ZK Proofs** (within the scope of **EVE Frontier**) for people without cryptography related knowledge or who want a quick explanation of what PODs are. For a more in-depth explanation, please look at [POD.org](https://pod.org/pod/introduction).

## Security Disclaimer

For information on what POD’s should and shouldn’t be used for, and a disclaimer about using them please visit [Disclaimers | pod.org](https://pod.org/gpc/disclaimers) before continuing.

## Introduction

POD, which stands for Provable Object Datatype, is a standard for cryptographic data created by [0xPARC](https://0xparc.org/). PODs allow you to store and share data in a secure, private, and verifiable way. Simply put, PODs are objects made of name / value pairs signed by their creator. The structure of PODs is optimized for privacy, integrity, and verifiability.

PODs mainly:

* Preserve the integrity of data (via digital signatures).
* Allow you to prove properties of the data, while optionally keeping some of it hidden (via zero knowledge proofs).
* Decentralize the data. This means users can hold onto their own PODs without having to rely on the game server to store them. For example, if jump data from a race is no longer available, players are still able to prove they won a race.

## Verify Data

Data can be verified, since when PODs are created, they get cryptographically signed by the creator of that POD (called the issuer) using their signing key. Using the signature and the public signing key, you can later confirm that:

* None of the data has been altered.
* It was created by the known creator (Either EVE Frontier/CCP Games or a known builder for example).

PODs are **JSON** based and have a structure such as below. This is based off a jump POD (link to the schema) which you can get from the game website.

```bash
{
    "entries": {
        "destinationId": 30018092,
        "shipId": 1741297384412,
        "originId": 30018090,
        "time": {
            "date": "2025-03-06T21:43:04.412Z"
        },
        "pod_type": "evefrontier.jump"
    },
    "signature": "N2xCm3tQzaqbBXfyNkb+/XAdHCpNDi1mZq0XWCOOahetZyW3274EQ/wUggQU5d5oUyM7T7MXj92w7+dEPt1lAA",
    "signerPublicKey": "hOLdtsUe3ieZsmBucDLJQi3kHe53zG6ZNDwypd3QZSo"
}
```

An example of how this could be useful, is for an in-game race in EVE Frontier.

## The Race

A corporation creates a race to get to a specific system in the game from another specific system. They want to know that people reached the system, without having to create SSU’s (Smart Storage Units) so they use PODs.

Participants after reaching the end of the race go to their account on [evefrontier.com](https://evefrontier.com/en) and export the starting system and the system jumps. They then submit that to the corporation through either a corporation created automated system or simply sending the data to the corporation leaders.

The corporation is then able to confirm that the participant:

* Left the starting system at or after the starting time
* Arrived at the end system at X time

## Hide Information

With PODs you can also hide the original information, while still being able to verify the data. This is called creating Zero Knowledge (ZK) Proofs. Zero knowledge proofs allow you to cryptographically prove statements about your private data without revealing anything else.

You can set ranges and criteria which are verified. An example of this could be hiding information in a killmail POD for a bounty board system.

## Bounty Board System

A builder sets up a bounty board system which allows players to collect bounties on other players. The builder doesn’t need to know who killed them or the solar system it happened in, just the killed player ID and that the kill happened after the bounty was created. This information is available in the killmail PODs (link to schema below).

The player generates a POD through the [http://evefrontier.com](http://evefrontier.com/) website, and then generates a ZK (Zero Knowledge) proof that has the killer ID, killer address, solar system ID and time hidden. The proof configuration looks similar to:

```bash
{
  pods: {
    killmail: {
      entries: {
        victimID: { isRevealed: true },
        victimAddress: { isRevealed: true },
        time: {
          isRevealed: false,
          inRange: { min: BOUNTY_CREATED_TIME, max: BOUNTY_EXPIRATION_TIME }
        }
      }
    }
  }
}
```

All parts of the killmail POD not specifically revealed in the configuration are hidden, but the ZK proof ensures that the prover has a valid POD signed by the official issuer. More information on proving and hiding information can be found here

## Why PODs?

PODs allow for data preservation and integrity whilst also allowing for privacy with hiding certain data. Other solutions may require you allow corporations access to your data at anytime, lowering privacy.

## What can I get PODs of for EVE Frontier?

You can make a POD from almost any data structured as names and values. Currently arrays / lists and nesting are not supported. For more detail on POD content see [Values and Types | pod.org](https://pod.org/pod/values).

We provide some PODs through our World API, or website with the entries including:

### Authenticated

### Gate Jumps

```bash
{
	"destinationID": "int",
	"originID": "int",
	"shipID": "int",
	"time": "dateTime",
}
```

### Killmails

```bash
{
	"victimID": "int",
	"victimAddress": "string"
	"killerID": "int",
	"killerAddress": "string",
	"solarSystemID": "int",
	"time": "dateTime"
}
```

## Public

### Types (Items)

```bash
{
	"typeID": "int",
	"smartItemID": "int",
	"description": "string",
	"attributes": "attributesObject"
}
```

### Solar Systems

```bash
{
	"systemID": "int",
	"regionID": "int",
	"name": "string",
	"location": {
		"x": "int",
		"y": "int",
		"z": "int"
	}
}
```

### Market Data

```bash
{
	"highestPrice": "float",
	"averagePrice": "float",
	"lowestPrice": "float",
	"orderQuantity": "int",
	"date": "dateTime"
}
```

In the future, we will have additional PODs available to players.

## Technical Guide to using PODs

### Creation

If you want to create your own POD with custom data, for example if you want to create a security level badge for your corporation to secure certain services and actions, then you can use the [Creating and Verifying](../creating-and-verifying/) guide.

### Verification

The easiest way to verify a POD for EVE Frontier is by using our API. You can simply POST to https://blockchain-gateway-stillness.live.tech.evefrontier.com/pod/verify with the POD in the body of the request and you can get verification information.

You can verify PODs from outside of EVE Frontier using the [Creating and Verifying](../creating-and-verifying/) guide.
