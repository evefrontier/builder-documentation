# Zero Knowlege Proofs

import { Steps } from "nextra/components"
import { Tabs, Tab } from "nextra/components"

import { Callout } from "nextra/components"; 
import { CollapseCode } from "../components/CollapseCode";

This guide will cover how Zero Knowledge Proofs of PODs operate from a builder perspective, and how to create and verify them.

<Callout emoji="⚠️">
Documentation around PODs is currently a work in progress.
</Callout>

## Pre-Requisites

### Required

Ensure you setup your tools using the [Setting up your tools](/Tools) guide, as you will need to use some of them to create and verify PODs and ZK proofs.

### Optional

We would suggest that you read the [Introduction to PODs](/pods) page and [Creating and Verifying PODs](/creating-and-verifying-pods) first if you haven't already, as it explains what **PODs** and how to create them.

## Code

You can either follow along with the code or use the [POD directory](https://github.com/projectawakening/builder-examples/tree/develop/pods) of the [Builder Examples](https://github.com/projectawakening/builder-examples/) which already has the code.

```bash copy copy
cd builder-examples/pods
```

## Zero Knowledge Proofs

Zero Knowledge Proofs allow you to hide information, while still being able to verify the information that is revealed and prove information that is not (within the set criteria). A proof might selectively reveal some (but not all) data in POD, or might reveal nothing other than some constraints on data (such as age ≥ 21 on a ID).

You can create Zero Knowledge Proofs (Also known as ZK Proofs) from PODs.

## GPC (General Purpose Circuits)

GPC, which stand for General Purpose Circuits is part of the POD standard for cryptographic data and is built by [0xPARC](https://0xparc.org/).

In zero-knowledge proofs (ZKPs), a "circuit" is essentially a specialized program. It encodes the specific rules (often called "constraints") that a piece of secret data must meet. A prover uses the circuit to generate a cryptographic proof that they possess data meeting these rules, without revealing the actual data itself.  A proof is just a collection of big numbers, but they are selected to be efficient to verify, but computationally infeasible to generate unless the prover has access to real PODs which satisfy the constraints.

### The Problem

Traditionally, creating these custom ZK proof circuits for every new set of rules is difficult and time-consuming.

### Why GPC can be efficient

GPC allows generating many different proofs using *configurable*, pre-existing circuits, avoiding the need to build new ones from scratch. This makes working with proofs faster and more flexible.

Generating proofs costs computing power, especially with larger circuits. The GPC compiler automatically chooses the smallest suitable circuit from a family to balance proof requirements and efficiency.

See pod.org/gpc/introduction for more information about GPCs.

### Jump Example (POD From EVE Frontier)

An example of this could with Jump PODs. If you want to prove that you travelled to the solar system before a certain time but not share the exact time, then you could generate a proof of the jump POD that you get from EVE Frontier, since you can create proofs of PODs that you did not create. 

If the input POD looks like:

- Source System ID
- Destination System ID
- Ship ID
- Date + Time

Then the ZK proof could have the destination system ID and ship ID, whilst having all the other data hidden. The proof config would look something like this in TypeScript:

```bash copy copy
{
  pods: {
    jump: {
      entries: {
        sourceSystemID: { isRevealed: false },
        destinationSystemID: { isRevealed: true },
        shipID: { isRevealed: true },
        time: {
          isRevealed: false,
          inRange: { min: 0n, max: SPECIFIC_BEFORE_TIME }
        }
      }
    }
  }
}
```

Using the ZK system, you could add a verification range to the POD so that people are able to verify that went to the system before the certain time without knowing the exact time.

### Corporation Security Access Badge Example (Custom POD from Corp)

An example of a custom POD could be a corporation security access badge. A corporation could create PODs for people in the corporation to allow members to get access to certain DApps, systems or events.

If the input POD looks like:

- Clearance Level
- Holder Smart Character Address
- Issued Date
- Expiry Date

Then the ZK proof could have all information other than the holder smart character address hidden. You could prove that:

- You have a high enough level without giving away what your level is
- The badge is valid, or also valid for another X months.

Having this off-chain and as a ZK proof allows privacy within and external to the corporation.

## Creating a ZK Proof of a POD

<Steps>

### Step 1: Setup your Project

Firstly, in the directory you want to use install the POD library with:

```bash copy
pnpm i @pcd/pod
```

Then, install tsx to easily run the typescript with:

```bash copy
pnpm i tsx
```

Then, install the GPC library with:

```bash copy
pnpm i @pcd/gpc
```

Then, create a file called **zkCreate.ts** and open it with your preferred IDE.

### Step 2: Import Packages

Import the packages required to create and verify PODs. Also import the GPC package functions for creating the ZK Proof.

<CollapseCode>
    ``` javascript copy showLineNumbers filename="zkCreate.ts" {2-9}
    //Import Packages
    import { POD, PODEntries, JSONPOD, PODValue, podValueFromJSON, deriveSignerPublicKey } from "@pcd/pod";

    import {
        gpcArtifactDownloadURL,
        GPCProofConfig, gpcProve,
        gpcVerify,
        boundConfigToJSON, revealedClaimsToJSON 
    } from "@pcd/gpc";

    //POD Data
    const myEntries: PODEntries = {
        security_level: {
            type: "int",
            value: 4n
        },
        holder_smart_character_address: {
            type: "string",
            value: "0x6d11ac8f376b6284a7e5d62a340f71869b3063ae"
        },
        issued_date: {
            type: "date",
            value: new Date("2025-04-10T00:00:00.000Z")
        },
        expiry_date: {
            type: "date",
            value: new Date("2026-04-10T00:00:00.000Z")
        },
        pod_type: { type: "string", value: "corpName.security_badge" },
    };

    //Your PRIVATE signing key
    const privateSigningKey = "2851153af6e862439ff91253684f85a6357ec7a3edcec4324de1eb7db4431ea5";

    //Create the POD
    const myPOD = POD.sign(myEntries, privateSigningKey);

    //Output Signer Public Key
    const publicSigningKey = deriveSignerPublicKey(privateSigningKey);
    console.log("\nSigner Public Key")
    console.log(publicSigningKey + "\n")

    //Import the GPC Artifacts
    const GPC_ARTIFACTS_PATH = "./node_modules/@pcd/proto-pod-gpc-artifacts";

    //Create the Proof Config
    const proofConfig: GPCProofConfig = {
        pods: {
            security_badge: {
                entries: {
                    security_level: { 
                        isRevealed: false,
                        inRange: {
                            min: 3n,
                            max: 10n
                        }
                    },
                    holder_smart_character_address: { isRevealed: true },
                    issued_date: {
                        isRevealed: false,
                        inRange: {
                            min: 0n,
                            max: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime())
                        }
                    },
                    expiry_date: {
                        isRevealed: false,
                        inRange: {
                            min: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime()),
                            max: BigInt(new Date("2030-04-10T00:00:00.000Z").getTime())
                        }
                    },
                    pod_type: { isRevealed: true }
                }
            }
        }
    };

    const proofInputs = {
        pods: {
            security_badge: myPOD
        }
    }

    async function CreateProof(){
        //Create the proof
        const { proof, boundConfig, revealedClaims } = await gpcProve(
            proofConfig,
            proofInputs,
            GPC_ARTIFACTS_PATH
        );

        //Convert proof information to JSON
        const proofMessage = JSON.stringify({
            proof: proof,
            boundConfig: boundConfigToJSON(boundConfig),
            revealedClaims: revealedClaimsToJSON(revealedClaims)
        });

        //Output the proof
        console.log(proofMessage)
    }

    CreateProof()
    ```
</CollapseCode>

### Step 3: Create the POD Data

Next, you create the data for the POD. This is the same data as the [Creating and Verifying PODs](/creating-and-verifying-pods) example.

<CollapseCode>
    ``` javascript copy showLineNumbers filename="zkCreate.ts" {12-30}
    //Import Packages
    import { POD, PODEntries, JSONPOD, PODValue, podValueFromJSON, deriveSignerPublicKey } from "@pcd/pod";

    import {
        gpcArtifactDownloadURL,
        GPCProofConfig, gpcProve,
        gpcVerify,
        boundConfigToJSON, revealedClaimsToJSON 
    } from "@pcd/gpc";

    //POD Data
    const myEntries: PODEntries = {
        security_level: {
            type: "int",
            value: 4n
        },
        holder_smart_character_address: {
            type: "string",
            value: "0x6d11ac8f376b6284a7e5d62a340f71869b3063ae"
        },
        issued_date: {
            type: "date",
            value: new Date("2025-04-10T00:00:00.000Z")
        },
        expiry_date: {
            type: "date",
            value: new Date("2026-04-10T00:00:00.000Z")
        },
        pod_type: { type: "string", value: "corpName.security_badge" },
    };

    //Your PRIVATE signing key
    const privateSigningKey = "2851153af6e862439ff91253684f85a6357ec7a3edcec4324de1eb7db4431ea5";

    //Create the POD
    const myPOD = POD.sign(myEntries, privateSigningKey);

    //Output Signer Public Key
    const publicSigningKey = deriveSignerPublicKey(privateSigningKey);
    console.log("\nSigner Public Key")
    console.log(publicSigningKey + "\n")

    //Import the GPC Artifacts
    const GPC_ARTIFACTS_PATH = "./node_modules/@pcd/proto-pod-gpc-artifacts";

    //Create the Proof Config
    const proofConfig: GPCProofConfig = {
        pods: {
            security_badge: {
                entries: {
                    security_level: { 
                        isRevealed: false,
                        inRange: {
                            min: 3n,
                            max: 10n
                        }
                    },
                    holder_smart_character_address: { isRevealed: true },
                    issued_date: {
                        isRevealed: false,
                        inRange: {
                            min: 0n,
                            max: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime())
                        }
                    },
                    expiry_date: {
                        isRevealed: false,
                        inRange: {
                            min: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime()),
                            max: BigInt(new Date("2030-04-10T00:00:00.000Z").getTime())
                        }
                    },
                    pod_type: { isRevealed: true }
                }
            }
        }
    };

    const proofInputs = {
        pods: {
            security_badge: myPOD
        }
    }

    async function CreateProof(){
        //Create the proof
        const { proof, boundConfig, revealedClaims } = await gpcProve(
            proofConfig,
            proofInputs,
            GPC_ARTIFACTS_PATH
        );

        //Convert proof information to JSON
        const proofMessage = JSON.stringify({
            proof: proof,
            boundConfig: boundConfigToJSON(boundConfig),
            revealedClaims: revealedClaimsToJSON(revealedClaims)
        });

        //Output the proof
        console.log(proofMessage)
    }

    CreateProof()
    ```
</CollapseCode>

### Step 4: Create and Sign the POD

Next, create the POD by signing the POD data with your private key. Often, the person creating the POD will be a different person to the person creating the proof but in this example, you will be doing both. 

<Callout emoji="⚠️">
Do not share your Private Key. This private key is for demonstration and should not be used in a live app.
</Callout>

<CollapseCode>
    ``` javascript copy showLineNumbers filename="zkCreate.ts" {33-36}
    //Import Packages
    import { POD, PODEntries, JSONPOD, PODValue, podValueFromJSON, deriveSignerPublicKey } from "@pcd/pod";

    import {
        gpcArtifactDownloadURL,
        GPCProofConfig, gpcProve,
        gpcVerify,
        boundConfigToJSON, revealedClaimsToJSON 
    } from "@pcd/gpc";

    //POD Data
    const myEntries: PODEntries = {
        security_level: {
            type: "int",
            value: 4n
        },
        holder_smart_character_address: {
            type: "string",
            value: "0x6d11ac8f376b6284a7e5d62a340f71869b3063ae"
        },
        issued_date: {
            type: "date",
            value: new Date("2025-04-10T00:00:00.000Z")
        },
        expiry_date: {
            type: "date",
            value: new Date("2026-04-10T00:00:00.000Z")
        },
        pod_type: { type: "string", value: "corpName.security_badge" },
    };

    //Your PRIVATE signing key
    const privateSigningKey = "2851153af6e862439ff91253684f85a6357ec7a3edcec4324de1eb7db4431ea5";

    //Create the POD
    const myPOD = POD.sign(myEntries, privateSigningKey);

    //Output Signer Public Key
    const publicSigningKey = deriveSignerPublicKey(privateSigningKey);
    console.log("\nSigner Public Key")
    console.log(publicSigningKey + "\n")

    //Import the GPC Artifacts
    const GPC_ARTIFACTS_PATH = "./node_modules/@pcd/proto-pod-gpc-artifacts";

    //Create the Proof Config
    const proofConfig: GPCProofConfig = {
        pods: {
            security_badge: {
                entries: {
                    security_level: { 
                        isRevealed: false,
                        inRange: {
                            min: 3n,
                            max: 10n
                        }
                    },
                    holder_smart_character_address: { isRevealed: true },
                    issued_date: {
                        isRevealed: false,
                        inRange: {
                            min: 0n,
                            max: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime())
                        }
                    },
                    expiry_date: {
                        isRevealed: false,
                        inRange: {
                            min: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime()),
                            max: BigInt(new Date("2030-04-10T00:00:00.000Z").getTime())
                        }
                    },
                    pod_type: { isRevealed: true }
                }
            }
        }
    };

    const proofInputs = {
        pods: {
            security_badge: myPOD
        }
    }

    async function CreateProof(){
        //Create the proof
        const { proof, boundConfig, revealedClaims } = await gpcProve(
            proofConfig,
            proofInputs,
            GPC_ARTIFACTS_PATH
        );

        //Convert proof information to JSON
        const proofMessage = JSON.stringify({
            proof: proof,
            boundConfig: boundConfigToJSON(boundConfig),
            revealedClaims: revealedClaimsToJSON(revealedClaims)
        });

        //Output the proof
        console.log(proofMessage)
    }

    CreateProof()
    ```
</CollapseCode>

### Step 5: Export Public Signing Key (Optional)

Then, you can export the public signing key for later use.

<CollapseCode>
    ``` javascript copy showLineNumbers filename="zkCreate.ts" {39-41}
    //Import Packages
    import { POD, PODEntries, JSONPOD, PODValue, podValueFromJSON, deriveSignerPublicKey } from "@pcd/pod";

    import {
        gpcArtifactDownloadURL,
        GPCProofConfig, gpcProve,
        gpcVerify,
        boundConfigToJSON, revealedClaimsToJSON 
    } from "@pcd/gpc";

    //POD Data
    const myEntries: PODEntries = {
        security_level: {
            type: "int",
            value: 4n
        },
        holder_smart_character_address: {
            type: "string",
            value: "0x6d11ac8f376b6284a7e5d62a340f71869b3063ae"
        },
        issued_date: {
            type: "date",
            value: new Date("2025-04-10T00:00:00.000Z")
        },
        expiry_date: {
            type: "date",
            value: new Date("2026-04-10T00:00:00.000Z")
        },
        pod_type: { type: "string", value: "corpName.security_badge" },
    };

    //Your PRIVATE signing key
    const privateSigningKey = "2851153af6e862439ff91253684f85a6357ec7a3edcec4324de1eb7db4431ea5";

    //Create the POD
    const myPOD = POD.sign(myEntries, privateSigningKey);

    //Output Signer Public Key
    const publicSigningKey = deriveSignerPublicKey(privateSigningKey);
    console.log("\nSigner Public Key")
    console.log(publicSigningKey + "\n")

    //Import the GPC Artifacts
    const GPC_ARTIFACTS_PATH = "./node_modules/@pcd/proto-pod-gpc-artifacts";

    //Create the Proof Config
    const proofConfig: GPCProofConfig = {
        pods: {
            security_badge: {
                entries: {
                    security_level: { 
                        isRevealed: false,
                        inRange: {
                            min: 3n,
                            max: 10n
                        }
                    },
                    holder_smart_character_address: { isRevealed: true },
                    issued_date: {
                        isRevealed: false,
                        inRange: {
                            min: 0n,
                            max: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime())
                        }
                    },
                    expiry_date: {
                        isRevealed: false,
                        inRange: {
                            min: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime()),
                            max: BigInt(new Date("2030-04-10T00:00:00.000Z").getTime())
                        }
                    },
                    pod_type: { isRevealed: true }
                }
            }
        }
    };

    const proofInputs = {
        pods: {
            security_badge: myPOD
        }
    }

    async function CreateProof(){
        //Create the proof
        const { proof, boundConfig, revealedClaims } = await gpcProve(
            proofConfig,
            proofInputs,
            GPC_ARTIFACTS_PATH
        );

        //Convert proof information to JSON
        const proofMessage = JSON.stringify({
            proof: proof,
            boundConfig: boundConfigToJSON(boundConfig),
            revealedClaims: revealedClaimsToJSON(revealedClaims)
        });

        //Output the proof
        console.log(proofMessage)
    }

    CreateProof()
    ```
</CollapseCode>

### Step 6: GPC Config

This step will depend on whether you are using the code on a website. In the example, it’s for a desktop program but you can also fetch the URL for web using the code from the dropdown below.

<details>

<summary>Web Code</summary>

```tsx
const artifactURL = gpcArtifactDownloadURL(
  "jsdelivr",
  "prod",
  undefined
)
```

Then use this instead of **GPC_ARTIFACTS_PATH**. 
</details>

Firstly, set the directory that you get the local artifact files from. These files define how the data is stored / hidden whilst still being able to be verified for specific criteria.

 Each proof will use one of the circuits in the family (chosen automatically)
- Artifacts include a proving key and verification key for each circuit
- It’s important for the prover and verifier to agree on the same set of artifacts to be compatible.

<CollapseCode>
    ``` javascript copy showLineNumbers filename="zkCreate.ts" {44}
    //Import Packages
    import { POD, PODEntries, JSONPOD, PODValue, podValueFromJSON, deriveSignerPublicKey } from "@pcd/pod";

    import {
        gpcArtifactDownloadURL,
        GPCProofConfig, gpcProve,
        gpcVerify,
        boundConfigToJSON, revealedClaimsToJSON 
    } from "@pcd/gpc";

    //POD Data
    const myEntries: PODEntries = {
        security_level: {
            type: "int",
            value: 4n
        },
        holder_smart_character_address: {
            type: "string",
            value: "0x6d11ac8f376b6284a7e5d62a340f71869b3063ae"
        },
        issued_date: {
            type: "date",
            value: new Date("2025-04-10T00:00:00.000Z")
        },
        expiry_date: {
            type: "date",
            value: new Date("2026-04-10T00:00:00.000Z")
        },
        pod_type: { type: "string", value: "corpName.security_badge" },
    };

    //Your PRIVATE signing key
    const privateSigningKey = "2851153af6e862439ff91253684f85a6357ec7a3edcec4324de1eb7db4431ea5";

    //Create the POD
    const myPOD = POD.sign(myEntries, privateSigningKey);

    //Output Signer Public Key
    const publicSigningKey = deriveSignerPublicKey(privateSigningKey);
    console.log("\nSigner Public Key")
    console.log(publicSigningKey + "\n")

    //Import the GPC Artifacts
    const GPC_ARTIFACTS_PATH = "./node_modules/@pcd/proto-pod-gpc-artifacts";

    //Create the Proof Config
    const proofConfig: GPCProofConfig = {
        pods: {
            security_badge: {
                entries: {
                    security_level: { 
                        isRevealed: false,
                        inRange: {
                            min: 3n,
                            max: 10n
                        }
                    },
                    holder_smart_character_address: { isRevealed: true },
                    issued_date: {
                        isRevealed: false,
                        inRange: {
                            min: 0n,
                            max: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime())
                        }
                    },
                    expiry_date: {
                        isRevealed: false,
                        inRange: {
                            min: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime()),
                            max: BigInt(new Date("2030-04-10T00:00:00.000Z").getTime())
                        }
                    },
                    pod_type: { isRevealed: true }
                }
            }
        }
    };

    const proofInputs = {
        pods: {
            security_badge: myPOD
        }
    }

    async function CreateProof(){
        //Create the proof
        const { proof, boundConfig, revealedClaims } = await gpcProve(
            proofConfig,
            proofInputs,
            GPC_ARTIFACTS_PATH
        );

        //Convert proof information to JSON
        const proofMessage = JSON.stringify({
            proof: proof,
            boundConfig: boundConfigToJSON(boundConfig),
            revealedClaims: revealedClaimsToJSON(revealedClaims)
        });

        //Output the proof
        console.log(proofMessage)
    }

    CreateProof()
    ```
</CollapseCode>

Then, create the GPC config. Since we want the badge name and holder to be public we set them to be revealed.

<CollapseCode>
    ``` javascript copy showLineNumbers filename="zkCreate.ts" {47-77}
    //Import Packages
    import { POD, PODEntries, JSONPOD, PODValue, podValueFromJSON, deriveSignerPublicKey } from "@pcd/pod";

    import {
        gpcArtifactDownloadURL,
        GPCProofConfig, gpcProve,
        gpcVerify,
        boundConfigToJSON, revealedClaimsToJSON 
    } from "@pcd/gpc";

    //POD Data
    const myEntries: PODEntries = {
        security_level: {
            type: "int",
            value: 4n
        },
        holder_smart_character_address: {
            type: "string",
            value: "0x6d11ac8f376b6284a7e5d62a340f71869b3063ae"
        },
        issued_date: {
            type: "date",
            value: new Date("2025-04-10T00:00:00.000Z")
        },
        expiry_date: {
            type: "date",
            value: new Date("2026-04-10T00:00:00.000Z")
        },
        pod_type: { type: "string", value: "corpName.security_badge" },
    };

    //Your PRIVATE signing key
    const privateSigningKey = "2851153af6e862439ff91253684f85a6357ec7a3edcec4324de1eb7db4431ea5";

    //Create the POD
    const myPOD = POD.sign(myEntries, privateSigningKey);

    //Output Signer Public Key
    const publicSigningKey = deriveSignerPublicKey(privateSigningKey);
    console.log("\nSigner Public Key")
    console.log(publicSigningKey + "\n")

    //Import the GPC Artifacts
    const GPC_ARTIFACTS_PATH = "./node_modules/@pcd/proto-pod-gpc-artifacts";

    //Create the Proof Config
    const proofConfig: GPCProofConfig = {
        pods: {
            security_badge: {
                entries: {
                    security_level: { 
                        isRevealed: false,
                        inRange: {
                            min: 3n,
                            max: 10n
                        }
                    },
                    holder_smart_character_address: { isRevealed: true },
                    issued_date: {
                        isRevealed: false,
                        inRange: {
                            min: 0n,
                            max: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime())
                        }
                    },
                    expiry_date: {
                        isRevealed: false,
                        inRange: {
                            min: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime()),
                            max: BigInt(new Date("2030-04-10T00:00:00.000Z").getTime())
                        }
                    },
                    pod_type: { isRevealed: true }
                }
            }
        }
    };

    const proofInputs = {
        pods: {
            security_badge: myPOD
        }
    }

    async function CreateProof(){
        //Create the proof
        const { proof, boundConfig, revealedClaims } = await gpcProve(
            proofConfig,
            proofInputs,
            GPC_ARTIFACTS_PATH
        );

        //Convert proof information to JSON
        const proofMessage = JSON.stringify({
            proof: proof,
            boundConfig: boundConfigToJSON(boundConfig),
            revealedClaims: revealedClaimsToJSON(revealedClaims)
        });

        //Output the proof
        console.log(proofMessage)
    }

    CreateProof()
    ```
</CollapseCode>

Then, create a object to define that we want to input the previously created POD to create the ZK proof.


<CollapseCode>
    ``` javascript copy showLineNumbers filename="zkCreate.ts" {79-83}
    //Import Packages
    import { POD, PODEntries, JSONPOD, PODValue, podValueFromJSON, deriveSignerPublicKey } from "@pcd/pod";

    import {
        gpcArtifactDownloadURL,
        GPCProofConfig, gpcProve,
        gpcVerify,
        boundConfigToJSON, revealedClaimsToJSON 
    } from "@pcd/gpc";

    //POD Data
    const myEntries: PODEntries = {
        security_level: {
            type: "int",
            value: 4n
        },
        holder_smart_character_address: {
            type: "string",
            value: "0x6d11ac8f376b6284a7e5d62a340f71869b3063ae"
        },
        issued_date: {
            type: "date",
            value: new Date("2025-04-10T00:00:00.000Z")
        },
        expiry_date: {
            type: "date",
            value: new Date("2026-04-10T00:00:00.000Z")
        },
        pod_type: { type: "string", value: "corpName.security_badge" },
    };

    //Your PRIVATE signing key
    const privateSigningKey = "2851153af6e862439ff91253684f85a6357ec7a3edcec4324de1eb7db4431ea5";

    //Create the POD
    const myPOD = POD.sign(myEntries, privateSigningKey);

    //Output Signer Public Key
    const publicSigningKey = deriveSignerPublicKey(privateSigningKey);
    console.log("\nSigner Public Key")
    console.log(publicSigningKey + "\n")

    //Import the GPC Artifacts
    const GPC_ARTIFACTS_PATH = "./node_modules/@pcd/proto-pod-gpc-artifacts";

    //Create the Proof Config
    const proofConfig: GPCProofConfig = {
        pods: {
            security_badge: {
                entries: {
                    security_level: { 
                        isRevealed: false,
                        inRange: {
                            min: 3n,
                            max: 10n
                        }
                    },
                    holder_smart_character_address: { isRevealed: true },
                    issued_date: {
                        isRevealed: false,
                        inRange: {
                            min: 0n,
                            max: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime())
                        }
                    },
                    expiry_date: {
                        isRevealed: false,
                        inRange: {
                            min: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime()),
                            max: BigInt(new Date("2030-04-10T00:00:00.000Z").getTime())
                        }
                    },
                    pod_type: { isRevealed: true }
                }
            }
        }
    };

    const proofInputs = {
        pods: {
            security_badge: myPOD
        }
    }

    async function CreateProof(){
        //Create the proof
        const { proof, boundConfig, revealedClaims } = await gpcProve(
            proofConfig,
            proofInputs,
            GPC_ARTIFACTS_PATH
        );

        //Convert proof information to JSON
        const proofMessage = JSON.stringify({
            proof: proof,
            boundConfig: boundConfigToJSON(boundConfig),
            revealedClaims: revealedClaimsToJSON(revealedClaims)
        });

        //Output the proof
        console.log(proofMessage)
    }

    CreateProof()
    ```
</CollapseCode>

### Step 7: Generate the Proof

Next, generate the proof.

<CollapseCode>
    ``` javascript copy showLineNumbers filename="zkCreate.ts" {87-91}
    //Import Packages
    import { POD, PODEntries, JSONPOD, PODValue, podValueFromJSON, deriveSignerPublicKey } from "@pcd/pod";

    import {
        gpcArtifactDownloadURL,
        GPCProofConfig, gpcProve,
        gpcVerify,
        boundConfigToJSON, revealedClaimsToJSON 
    } from "@pcd/gpc";

    //POD Data
    const myEntries: PODEntries = {
        security_level: {
            type: "int",
            value: 4n
        },
        holder_smart_character_address: {
            type: "string",
            value: "0x6d11ac8f376b6284a7e5d62a340f71869b3063ae"
        },
        issued_date: {
            type: "date",
            value: new Date("2025-04-10T00:00:00.000Z")
        },
        expiry_date: {
            type: "date",
            value: new Date("2026-04-10T00:00:00.000Z")
        },
        pod_type: { type: "string", value: "corpName.security_badge" },
    };

    //Your PRIVATE signing key
    const privateSigningKey = "2851153af6e862439ff91253684f85a6357ec7a3edcec4324de1eb7db4431ea5";

    //Create the POD
    const myPOD = POD.sign(myEntries, privateSigningKey);

    //Output Signer Public Key
    const publicSigningKey = deriveSignerPublicKey(privateSigningKey);
    console.log("\nSigner Public Key")
    console.log(publicSigningKey + "\n")

    //Import the GPC Artifacts
    const GPC_ARTIFACTS_PATH = "./node_modules/@pcd/proto-pod-gpc-artifacts";

    //Create the Proof Config
    const proofConfig: GPCProofConfig = {
        pods: {
            security_badge: {
                entries: {
                    security_level: { 
                        isRevealed: false,
                        inRange: {
                            min: 3n,
                            max: 10n
                        }
                    },
                    holder_smart_character_address: { isRevealed: true },
                    issued_date: {
                        isRevealed: false,
                        inRange: {
                            min: 0n,
                            max: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime())
                        }
                    },
                    expiry_date: {
                        isRevealed: false,
                        inRange: {
                            min: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime()),
                            max: BigInt(new Date("2030-04-10T00:00:00.000Z").getTime())
                        }
                    },
                    pod_type: { isRevealed: true }
                }
            }
        }
    };

    const proofInputs = {
        pods: {
            security_badge: myPOD
        }
    }

    async function CreateProof(){
        //Create the proof
        const { proof, boundConfig, revealedClaims } = await gpcProve(
            proofConfig,
            proofInputs,
            GPC_ARTIFACTS_PATH
        );

        //Convert proof information to JSON
        const proofMessage = JSON.stringify({
            proof: proof,
            boundConfig: boundConfigToJSON(boundConfig),
            revealedClaims: revealedClaimsToJSON(revealedClaims)
        });

        //Output the proof
        console.log(proofMessage)
    }

    CreateProof()
    ```
</CollapseCode>

### Step 8: Export the Proof Data

Next, export the proof data. We also want to export the inputs so that others know what we are verifying.

<CollapseCode>
    ``` javascript copy showLineNumbers filename="zkCreate.ts" {94-101}
    //Import Packages
    import { POD, PODEntries, JSONPOD, PODValue, podValueFromJSON, deriveSignerPublicKey } from "@pcd/pod";

    import {
        gpcArtifactDownloadURL,
        GPCProofConfig, gpcProve,
        gpcVerify,
        boundConfigToJSON, revealedClaimsToJSON 
    } from "@pcd/gpc";

    //POD Data
    const myEntries: PODEntries = {
        security_level: {
            type: "int",
            value: 4n
        },
        holder_smart_character_address: {
            type: "string",
            value: "0x6d11ac8f376b6284a7e5d62a340f71869b3063ae"
        },
        issued_date: {
            type: "date",
            value: new Date("2025-04-10T00:00:00.000Z")
        },
        expiry_date: {
            type: "date",
            value: new Date("2026-04-10T00:00:00.000Z")
        },
        pod_type: { type: "string", value: "corpName.security_badge" },
    };

    //Your PRIVATE signing key
    const privateSigningKey = "2851153af6e862439ff91253684f85a6357ec7a3edcec4324de1eb7db4431ea5";

    //Create the POD
    const myPOD = POD.sign(myEntries, privateSigningKey);

    //Output Signer Public Key
    const publicSigningKey = deriveSignerPublicKey(privateSigningKey);
    console.log("\nSigner Public Key")
    console.log(publicSigningKey + "\n")

    //Import the GPC Artifacts
    const GPC_ARTIFACTS_PATH = "./node_modules/@pcd/proto-pod-gpc-artifacts";

    //Create the Proof Config
    const proofConfig: GPCProofConfig = {
        pods: {
            security_badge: {
                entries: {
                    security_level: { 
                        isRevealed: false,
                        inRange: {
                            min: 3n,
                            max: 10n
                        }
                    },
                    holder_smart_character_address: { isRevealed: true },
                    issued_date: {
                        isRevealed: false,
                        inRange: {
                            min: 0n,
                            max: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime())
                        }
                    },
                    expiry_date: {
                        isRevealed: false,
                        inRange: {
                            min: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime()),
                            max: BigInt(new Date("2030-04-10T00:00:00.000Z").getTime())
                        }
                    },
                    pod_type: { isRevealed: true }
                }
            }
        }
    };

    const proofInputs = {
        pods: {
            security_badge: myPOD
        }
    }

    async function CreateProof(){
        //Create the proof
        const { proof, boundConfig, revealedClaims } = await gpcProve(
            proofConfig,
            proofInputs,
            GPC_ARTIFACTS_PATH
        );

        //Convert proof information to JSON
        const proofMessage = JSON.stringify({
            proof: proof,
            boundConfig: boundConfigToJSON(boundConfig),
            revealedClaims: revealedClaimsToJSON(revealedClaims)
        });

        //Output the proof
        console.log(proofMessage)
    }

    CreateProof()
    ```
</CollapseCode>

### Step 9: Run the Code

You can now run the code using:

```bash copy copy
pnpm tsx create.ts
```

</Steps>

## Verifying a ZK Proof of a POD

<Steps>

### Step 1: File Setup

Firstly, create a file named **zkVerify.ts** in the same directory as **zkCreate.ts** and open it in your IDE of choice.

### Step 2: Import Packages

Import the POD package functions to get the POD from the proof. Also import the GPC package functions for verifying the ZK Proof.

<CollapseCode>
    ``` javascript copy showLineNumbers filename="zkVerify.ts" {2-6}
    //Import the GPC Packages
    import {
        GPCProofConfig, gpcVerify,
        boundConfigFromJSON, revealedClaimsFromJSON,
        GPCBoundConfig
    } from "@pcd/gpc";

    //Import the GPC Artifacts
    const GPC_ARTIFACTS_PATH = "./node_modules/@pcd/proto-pod-gpc-artifacts";

    //Import the Proof Config
    const expectedProofConfig: GPCProofConfig = {
        pods: {
            security_badge: {
                entries: {
                    security_level: { 
                        isRevealed: false,
                        inRange: {
                            min: 3n,
                            max: 10n
                        }
                    },
                    holder_smart_character_address: { isRevealed: true },
                    issued_date: {
                        isRevealed: false,
                        inRange: {
                            min: 0n,
                            max: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime())
                        }
                    },
                    expiry_date: {
                        isRevealed: false,
                        inRange: {
                            min: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime()),
                            max: BigInt(new Date("2030-04-10T00:00:00.000Z").getTime())
                        }
                    },
                    pod_type: { isRevealed: true }
                }
            }
        }
    };

    //Import the Proof Data
    const proofMessage = '{"proof":{"pi_a":["18382096760714031908670127575213136578083295521215433334251317660564033378720","21030477399437073370476149069090721170053337449725832469428119547769133404031","1"],"pi_b":[["3441945812976152446306624803438621070022957041244859610613891371824225138447","13484259047285811435888513023900289692797242279920846387677844428076348775051"],["5980242761206523890540011436691111069270009222756334744394997945384381875367","15263548166808880668931526071918599213486304986568041225530763198080863123460"],["1","0"]],"pi_c":["19436657118428907159797452603147484224001485778247802421550746896397384983856","3196080941424733966474612046643629155603406339625206953551307106366932306579","1"],"protocol":"groth16","curve":"bn128"},"boundConfig":{"circuitIdentifier":"proto-pod-gpc_1o-12e-5md-4nv-0ei-1x5l-0x0t-0ov3-1ov4","pods":{"security_badge":{"entries":{"expiry_date":{"isRevealed":false,"inRange":{"min":1746835200000,"max":1902009600000}},"holder_smart_character_address":{"isRevealed":true},"issued_date":{"isRevealed":false,"inRange":{"min":0,"max":1746835200000}},"pod_type":{"isRevealed":true},"security_level":{"isRevealed":false,"inRange":{"min":3,"max":10}}}}}},"revealedClaims":{"pods":{"security_badge":{"entries":{"holder_smart_character_address":"0x6d11ac8f376b6284a7e5d62a340f71869b3063ae","pod_type":"corpName.security_badge"},"signerPublicKey":"3iREOe5OdCEZ0KaF4pOfFc5nMvG6iZbY7GeaMy2P3xw"}}}}'

    //Parse the Proof Data to JSON
    const receivedFromProver = JSON.parse(proofMessage);

    //Get the Proof, Bound Config, and Revealed Claims
    const proof = receivedFromProver.proof;
    const boundConfig = boundConfigFromJSON(receivedFromProver.boundConfig);
    const revealedClaims = revealedClaimsFromJSON(receivedFromProver.revealedClaims);

    //Verify the Proof
    async function VerifyProof(){
        const verifyConfig: GPCBoundConfig = {
            ...expectedProofConfig,
            circuitIdentifier: boundConfig.circuitIdentifier
        }

        //Verify the Proof
        const isValid = await gpcVerify(
            proof,
            verifyConfig,
            revealedClaims,
            GPC_ARTIFACTS_PATH
        );

        //Print the result
        if(!isValid){
            throw new Error("Proof is invalid");   
        }

        console.log("Proof is valid");

        const officialPublicKey = "3iREOe5OdCEZ0KaF4pOfFc5nMvG6iZbY7GeaMy2P3xw"

        if(revealedClaims.pods.security_badge.signerPublicKey != officialPublicKey){
            throw new Error("Not the official signer");
        }

        const badgeEntries = revealedClaims.pods.security_badge.entries;

        const officialPodType = "corpName.security_badge"

        if(badgeEntries.pod_type.value !== officialPodType){
            throw new Error("Not the right type of POD");
        }

        console.log("Correct POD Type")

        const expectedHolderAddress = "0x6d11ac8f376b6284a7e5d62a340f71869b3063ae"

        const holderAddress = badgeEntries.holder_smart_character_address.value;

        if(holderAddress != expectedHolderAddress){
            throw new Error("Not the right holder address");
        }
            
        console.log("Verfied security badge for character", holderAddress);

        //Exit the program
        process.exit(0);
    }

    //Run the verification
    VerifyProof()
    ```
</CollapseCode>

### Step 3: Create the expected GPC Config

Import the **GPC_ARTIFACTS_PATH** and **expectedConfig** that you created from the previous creation process. The reason why you want to import a expected config is that you can then check that the proof which was generated is confirming the same criteria that you are wanting to check.

<CollapseCode>
    ``` javascript copy showLineNumbers filename="zkVerify.ts" {8-42}
    //Import the GPC Packages
    import {
        GPCProofConfig, gpcVerify,
        boundConfigFromJSON, revealedClaimsFromJSON,
        GPCBoundConfig
    } from "@pcd/gpc";

    //Import the GPC Artifacts
    const GPC_ARTIFACTS_PATH = "./node_modules/@pcd/proto-pod-gpc-artifacts";

    //Import the Proof Config
    const expectedProofConfig: GPCProofConfig = {
        pods: {
            security_badge: {
                entries: {
                    security_level: { 
                        isRevealed: false,
                        inRange: {
                            min: 3n,
                            max: 10n
                        }
                    },
                    holder_smart_character_address: { isRevealed: true },
                    issued_date: {
                        isRevealed: false,
                        inRange: {
                            min: 0n,
                            max: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime())
                        }
                    },
                    expiry_date: {
                        isRevealed: false,
                        inRange: {
                            min: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime()),
                            max: BigInt(new Date("2030-04-10T00:00:00.000Z").getTime())
                        }
                    },
                    pod_type: { isRevealed: true }
                }
            }
        }
    };

    //Import the Proof Data
    const proofMessage = '{"proof":{"pi_a":["18382096760714031908670127575213136578083295521215433334251317660564033378720","21030477399437073370476149069090721170053337449725832469428119547769133404031","1"],"pi_b":[["3441945812976152446306624803438621070022957041244859610613891371824225138447","13484259047285811435888513023900289692797242279920846387677844428076348775051"],["5980242761206523890540011436691111069270009222756334744394997945384381875367","15263548166808880668931526071918599213486304986568041225530763198080863123460"],["1","0"]],"pi_c":["19436657118428907159797452603147484224001485778247802421550746896397384983856","3196080941424733966474612046643629155603406339625206953551307106366932306579","1"],"protocol":"groth16","curve":"bn128"},"boundConfig":{"circuitIdentifier":"proto-pod-gpc_1o-12e-5md-4nv-0ei-1x5l-0x0t-0ov3-1ov4","pods":{"security_badge":{"entries":{"expiry_date":{"isRevealed":false,"inRange":{"min":1746835200000,"max":1902009600000}},"holder_smart_character_address":{"isRevealed":true},"issued_date":{"isRevealed":false,"inRange":{"min":0,"max":1746835200000}},"pod_type":{"isRevealed":true},"security_level":{"isRevealed":false,"inRange":{"min":3,"max":10}}}}}},"revealedClaims":{"pods":{"security_badge":{"entries":{"holder_smart_character_address":"0x6d11ac8f376b6284a7e5d62a340f71869b3063ae","pod_type":"corpName.security_badge"},"signerPublicKey":"3iREOe5OdCEZ0KaF4pOfFc5nMvG6iZbY7GeaMy2P3xw"}}}}'

    //Parse the Proof Data to JSON
    const receivedFromProver = JSON.parse(proofMessage);

    //Get the Proof, Bound Config, and Revealed Claims
    const proof = receivedFromProver.proof;
    const boundConfig = boundConfigFromJSON(receivedFromProver.boundConfig);
    const revealedClaims = revealedClaimsFromJSON(receivedFromProver.revealedClaims);

    //Verify the Proof
    async function VerifyProof(){
        const verifyConfig: GPCBoundConfig = {
            ...expectedProofConfig,
            circuitIdentifier: boundConfig.circuitIdentifier
        }

        //Verify the Proof
        const isValid = await gpcVerify(
            proof,
            verifyConfig,
            revealedClaims,
            GPC_ARTIFACTS_PATH
        );

        //Print the result
        if(!isValid){
            throw new Error("Proof is invalid");   
        }

        console.log("Proof is valid");

        const officialPublicKey = "3iREOe5OdCEZ0KaF4pOfFc5nMvG6iZbY7GeaMy2P3xw"

        if(revealedClaims.pods.security_badge.signerPublicKey != officialPublicKey){
            throw new Error("Not the official signer");
        }

        const badgeEntries = revealedClaims.pods.security_badge.entries;

        const officialPodType = "corpName.security_badge"

        if(badgeEntries.pod_type.value !== officialPodType){
            throw new Error("Not the right type of POD");
        }

        console.log("Correct POD Type")

        const expectedHolderAddress = "0x6d11ac8f376b6284a7e5d62a340f71869b3063ae"

        const holderAddress = badgeEntries.holder_smart_character_address.value;

        if(holderAddress != expectedHolderAddress){
            throw new Error("Not the right holder address");
        }
            
        console.log("Verfied security badge for character", holderAddress);

        //Exit the program
        process.exit(0);
    }

    //Run the verification
    VerifyProof()
    ```
</CollapseCode>

### Step 4: Import Proof Data

Import the proof data that you created from the previous creation process.

<CollapseCode>
    ``` javascript copy showLineNumbers filename="zkVerify.ts" {45-53}
    //Import the GPC Packages
    import {
        GPCProofConfig, gpcVerify,
        boundConfigFromJSON, revealedClaimsFromJSON,
        GPCBoundConfig
    } from "@pcd/gpc";

    //Import the GPC Artifacts
    const GPC_ARTIFACTS_PATH = "./node_modules/@pcd/proto-pod-gpc-artifacts";

    //Import the Proof Config
    const expectedProofConfig: GPCProofConfig = {
        pods: {
            security_badge: {
                entries: {
                    security_level: { 
                        isRevealed: false,
                        inRange: {
                            min: 3n,
                            max: 10n
                        }
                    },
                    holder_smart_character_address: { isRevealed: true },
                    issued_date: {
                        isRevealed: false,
                        inRange: {
                            min: 0n,
                            max: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime())
                        }
                    },
                    expiry_date: {
                        isRevealed: false,
                        inRange: {
                            min: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime()),
                            max: BigInt(new Date("2030-04-10T00:00:00.000Z").getTime())
                        }
                    },
                    pod_type: { isRevealed: true }
                }
            }
        }
    };

    //Import the Proof Data
    const proofMessage = '{"proof":{"pi_a":["18382096760714031908670127575213136578083295521215433334251317660564033378720","21030477399437073370476149069090721170053337449725832469428119547769133404031","1"],"pi_b":[["3441945812976152446306624803438621070022957041244859610613891371824225138447","13484259047285811435888513023900289692797242279920846387677844428076348775051"],["5980242761206523890540011436691111069270009222756334744394997945384381875367","15263548166808880668931526071918599213486304986568041225530763198080863123460"],["1","0"]],"pi_c":["19436657118428907159797452603147484224001485778247802421550746896397384983856","3196080941424733966474612046643629155603406339625206953551307106366932306579","1"],"protocol":"groth16","curve":"bn128"},"boundConfig":{"circuitIdentifier":"proto-pod-gpc_1o-12e-5md-4nv-0ei-1x5l-0x0t-0ov3-1ov4","pods":{"security_badge":{"entries":{"expiry_date":{"isRevealed":false,"inRange":{"min":1746835200000,"max":1902009600000}},"holder_smart_character_address":{"isRevealed":true},"issued_date":{"isRevealed":false,"inRange":{"min":0,"max":1746835200000}},"pod_type":{"isRevealed":true},"security_level":{"isRevealed":false,"inRange":{"min":3,"max":10}}}}}},"revealedClaims":{"pods":{"security_badge":{"entries":{"holder_smart_character_address":"0x6d11ac8f376b6284a7e5d62a340f71869b3063ae","pod_type":"corpName.security_badge"},"signerPublicKey":"3iREOe5OdCEZ0KaF4pOfFc5nMvG6iZbY7GeaMy2P3xw"}}}}'

    //Parse the Proof Data to JSON
    const receivedFromProver = JSON.parse(proofMessage);

    //Get the Proof, Bound Config, and Revealed Claims
    const proof = receivedFromProver.proof;
    const boundConfig = boundConfigFromJSON(receivedFromProver.boundConfig);
    const revealedClaims = revealedClaimsFromJSON(receivedFromProver.revealedClaims);

    //Verify the Proof
    async function VerifyProof(){
        const verifyConfig: GPCBoundConfig = {
            ...expectedProofConfig,
            circuitIdentifier: boundConfig.circuitIdentifier
        }

        //Verify the Proof
        const isValid = await gpcVerify(
            proof,
            verifyConfig,
            revealedClaims,
            GPC_ARTIFACTS_PATH
        );

        //Print the result
        if(!isValid){
            throw new Error("Proof is invalid");   
        }

        console.log("Proof is valid");

        const officialPublicKey = "3iREOe5OdCEZ0KaF4pOfFc5nMvG6iZbY7GeaMy2P3xw"

        if(revealedClaims.pods.security_badge.signerPublicKey != officialPublicKey){
            throw new Error("Not the official signer");
        }

        const badgeEntries = revealedClaims.pods.security_badge.entries;

        const officialPodType = "corpName.security_badge"

        if(badgeEntries.pod_type.value !== officialPodType){
            throw new Error("Not the right type of POD");
        }

        console.log("Correct POD Type")

        const expectedHolderAddress = "0x6d11ac8f376b6284a7e5d62a340f71869b3063ae"

        const holderAddress = badgeEntries.holder_smart_character_address.value;

        if(holderAddress != expectedHolderAddress){
            throw new Error("Not the right holder address");
        }
            
        console.log("Verfied security badge for character", holderAddress);

        //Exit the program
        process.exit(0);
    }

    //Run the verification
    VerifyProof()
    ```
</CollapseCode>

### Step 5: Verify Proof

Create a GPC Bound Config using the previously created **expectedConfig** variable. This will direct the verification to use that proof to ensure the proof is proving the expected criteria.  All you need from the prover is the `circuitIdentifier` which indicates which circuit was selected from the family.

<CollapseCode>
    ``` javascript copy showLineNumbers filename="zkVerify.ts" {57-60}
    //Import the GPC Packages
    import {
        GPCProofConfig, gpcVerify,
        boundConfigFromJSON, revealedClaimsFromJSON,
        GPCBoundConfig
    } from "@pcd/gpc";

    //Import the GPC Artifacts
    const GPC_ARTIFACTS_PATH = "./node_modules/@pcd/proto-pod-gpc-artifacts";

    //Import the Proof Config
    const expectedProofConfig: GPCProofConfig = {
        pods: {
            security_badge: {
                entries: {
                    security_level: { 
                        isRevealed: false,
                        inRange: {
                            min: 3n,
                            max: 10n
                        }
                    },
                    holder_smart_character_address: { isRevealed: true },
                    issued_date: {
                        isRevealed: false,
                        inRange: {
                            min: 0n,
                            max: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime())
                        }
                    },
                    expiry_date: {
                        isRevealed: false,
                        inRange: {
                            min: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime()),
                            max: BigInt(new Date("2030-04-10T00:00:00.000Z").getTime())
                        }
                    },
                    pod_type: { isRevealed: true }
                }
            }
        }
    };

    //Import the Proof Data
    const proofMessage = '{"proof":{"pi_a":["18382096760714031908670127575213136578083295521215433334251317660564033378720","21030477399437073370476149069090721170053337449725832469428119547769133404031","1"],"pi_b":[["3441945812976152446306624803438621070022957041244859610613891371824225138447","13484259047285811435888513023900289692797242279920846387677844428076348775051"],["5980242761206523890540011436691111069270009222756334744394997945384381875367","15263548166808880668931526071918599213486304986568041225530763198080863123460"],["1","0"]],"pi_c":["19436657118428907159797452603147484224001485778247802421550746896397384983856","3196080941424733966474612046643629155603406339625206953551307106366932306579","1"],"protocol":"groth16","curve":"bn128"},"boundConfig":{"circuitIdentifier":"proto-pod-gpc_1o-12e-5md-4nv-0ei-1x5l-0x0t-0ov3-1ov4","pods":{"security_badge":{"entries":{"expiry_date":{"isRevealed":false,"inRange":{"min":1746835200000,"max":1902009600000}},"holder_smart_character_address":{"isRevealed":true},"issued_date":{"isRevealed":false,"inRange":{"min":0,"max":1746835200000}},"pod_type":{"isRevealed":true},"security_level":{"isRevealed":false,"inRange":{"min":3,"max":10}}}}}},"revealedClaims":{"pods":{"security_badge":{"entries":{"holder_smart_character_address":"0x6d11ac8f376b6284a7e5d62a340f71869b3063ae","pod_type":"corpName.security_badge"},"signerPublicKey":"3iREOe5OdCEZ0KaF4pOfFc5nMvG6iZbY7GeaMy2P3xw"}}}}'

    //Parse the Proof Data to JSON
    const receivedFromProver = JSON.parse(proofMessage);

    //Get the Proof, Bound Config, and Revealed Claims
    const proof = receivedFromProver.proof;
    const boundConfig = boundConfigFromJSON(receivedFromProver.boundConfig);
    const revealedClaims = revealedClaimsFromJSON(receivedFromProver.revealedClaims);

    //Verify the Proof
    async function VerifyProof(){
        const verifyConfig: GPCBoundConfig = {
            ...expectedProofConfig,
            circuitIdentifier: boundConfig.circuitIdentifier
        }

        //Verify the Proof
        const isValid = await gpcVerify(
            proof,
            verifyConfig,
            revealedClaims,
            GPC_ARTIFACTS_PATH
        );

        //Print the result
        if(!isValid){
            throw new Error("Proof is invalid");   
        }

        console.log("Proof is valid");

        const officialPublicKey = "3iREOe5OdCEZ0KaF4pOfFc5nMvG6iZbY7GeaMy2P3xw"

        if(revealedClaims.pods.security_badge.signerPublicKey != officialPublicKey){
            throw new Error("Not the official signer");
        }

        const badgeEntries = revealedClaims.pods.security_badge.entries;

        const officialPodType = "corpName.security_badge"

        if(badgeEntries.pod_type.value !== officialPodType){
            throw new Error("Not the right type of POD");
        }

        console.log("Correct POD Type")

        const expectedHolderAddress = "0x6d11ac8f376b6284a7e5d62a340f71869b3063ae"

        const holderAddress = badgeEntries.holder_smart_character_address.value;

        if(holderAddress != expectedHolderAddress){
            throw new Error("Not the right holder address");
        }
            
        console.log("Verfied security badge for character", holderAddress);

        //Exit the program
        process.exit(0);
    }

    //Run the verification
    VerifyProof()
    ```
</CollapseCode>

Next, verify the Proof using the **gpcVerify** function.

<CollapseCode>
    ``` javascript copy showLineNumbers filename="zkVerify.ts" {63-75}
    //Import the GPC Packages
    import {
        GPCProofConfig, gpcVerify,
        boundConfigFromJSON, revealedClaimsFromJSON,
        GPCBoundConfig
    } from "@pcd/gpc";

    //Import the GPC Artifacts
    const GPC_ARTIFACTS_PATH = "./node_modules/@pcd/proto-pod-gpc-artifacts";

    //Import the Proof Config
    const expectedProofConfig: GPCProofConfig = {
        pods: {
            security_badge: {
                entries: {
                    security_level: { 
                        isRevealed: false,
                        inRange: {
                            min: 3n,
                            max: 10n
                        }
                    },
                    holder_smart_character_address: { isRevealed: true },
                    issued_date: {
                        isRevealed: false,
                        inRange: {
                            min: 0n,
                            max: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime())
                        }
                    },
                    expiry_date: {
                        isRevealed: false,
                        inRange: {
                            min: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime()),
                            max: BigInt(new Date("2030-04-10T00:00:00.000Z").getTime())
                        }
                    },
                    pod_type: { isRevealed: true }
                }
            }
        }
    };

    //Import the Proof Data
    const proofMessage = '{"proof":{"pi_a":["18382096760714031908670127575213136578083295521215433334251317660564033378720","21030477399437073370476149069090721170053337449725832469428119547769133404031","1"],"pi_b":[["3441945812976152446306624803438621070022957041244859610613891371824225138447","13484259047285811435888513023900289692797242279920846387677844428076348775051"],["5980242761206523890540011436691111069270009222756334744394997945384381875367","15263548166808880668931526071918599213486304986568041225530763198080863123460"],["1","0"]],"pi_c":["19436657118428907159797452603147484224001485778247802421550746896397384983856","3196080941424733966474612046643629155603406339625206953551307106366932306579","1"],"protocol":"groth16","curve":"bn128"},"boundConfig":{"circuitIdentifier":"proto-pod-gpc_1o-12e-5md-4nv-0ei-1x5l-0x0t-0ov3-1ov4","pods":{"security_badge":{"entries":{"expiry_date":{"isRevealed":false,"inRange":{"min":1746835200000,"max":1902009600000}},"holder_smart_character_address":{"isRevealed":true},"issued_date":{"isRevealed":false,"inRange":{"min":0,"max":1746835200000}},"pod_type":{"isRevealed":true},"security_level":{"isRevealed":false,"inRange":{"min":3,"max":10}}}}}},"revealedClaims":{"pods":{"security_badge":{"entries":{"holder_smart_character_address":"0x6d11ac8f376b6284a7e5d62a340f71869b3063ae","pod_type":"corpName.security_badge"},"signerPublicKey":"3iREOe5OdCEZ0KaF4pOfFc5nMvG6iZbY7GeaMy2P3xw"}}}}'

    //Parse the Proof Data to JSON
    const receivedFromProver = JSON.parse(proofMessage);

    //Get the Proof, Bound Config, and Revealed Claims
    const proof = receivedFromProver.proof;
    const boundConfig = boundConfigFromJSON(receivedFromProver.boundConfig);
    const revealedClaims = revealedClaimsFromJSON(receivedFromProver.revealedClaims);

    //Verify the Proof
    async function VerifyProof(){
        const verifyConfig: GPCBoundConfig = {
            ...expectedProofConfig,
            circuitIdentifier: boundConfig.circuitIdentifier
        }

        //Verify the Proof
        const isValid = await gpcVerify(
            proof,
            verifyConfig,
            revealedClaims,
            GPC_ARTIFACTS_PATH
        );

        //Print the result
        if(!isValid){
            throw new Error("Proof is invalid");   
        }

        console.log("Proof is valid");

        const officialPublicKey = "3iREOe5OdCEZ0KaF4pOfFc5nMvG6iZbY7GeaMy2P3xw"

        if(revealedClaims.pods.security_badge.signerPublicKey != officialPublicKey){
            throw new Error("Not the official signer");
        }

        const badgeEntries = revealedClaims.pods.security_badge.entries;

        const officialPodType = "corpName.security_badge"

        if(badgeEntries.pod_type.value !== officialPodType){
            throw new Error("Not the right type of POD");
        }

        console.log("Correct POD Type")

        const expectedHolderAddress = "0x6d11ac8f376b6284a7e5d62a340f71869b3063ae"

        const holderAddress = badgeEntries.holder_smart_character_address.value;

        if(holderAddress != expectedHolderAddress){
            throw new Error("Not the right holder address");
        }
            
        console.log("Verfied security badge for character", holderAddress);

        //Exit the program
        process.exit(0);
    }

    //Run the verification
    VerifyProof()
    ```
</CollapseCode>

### Step 6: Verify the Public Key

Next, verify the public signing key from the proof.

Whilst it's not required to verify the public key, it's **strongly** recommended to ensure the POD was created from the correct source.  Anyone can sign a POD, so what makes it trustworthy is knowing who signed it.  Checking the `pod_type` (if present) is also good practice.

<CollapseCode>
    ``` javascript copy showLineNumbers filename="zkVerify.ts" {77-81}
    //Import the GPC Packages
    import {
        GPCProofConfig, gpcVerify,
        boundConfigFromJSON, revealedClaimsFromJSON,
        GPCBoundConfig
    } from "@pcd/gpc";

    //Import the GPC Artifacts
    const GPC_ARTIFACTS_PATH = "./node_modules/@pcd/proto-pod-gpc-artifacts";

    //Import the Proof Config
    const expectedProofConfig: GPCProofConfig = {
        pods: {
            security_badge: {
                entries: {
                    security_level: { 
                        isRevealed: false,
                        inRange: {
                            min: 3n,
                            max: 10n
                        }
                    },
                    holder_smart_character_address: { isRevealed: true },
                    issued_date: {
                        isRevealed: false,
                        inRange: {
                            min: 0n,
                            max: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime())
                        }
                    },
                    expiry_date: {
                        isRevealed: false,
                        inRange: {
                            min: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime()),
                            max: BigInt(new Date("2030-04-10T00:00:00.000Z").getTime())
                        }
                    },
                    pod_type: { isRevealed: true }
                }
            }
        }
    };

    //Import the Proof Data
    const proofMessage = '{"proof":{"pi_a":["18382096760714031908670127575213136578083295521215433334251317660564033378720","21030477399437073370476149069090721170053337449725832469428119547769133404031","1"],"pi_b":[["3441945812976152446306624803438621070022957041244859610613891371824225138447","13484259047285811435888513023900289692797242279920846387677844428076348775051"],["5980242761206523890540011436691111069270009222756334744394997945384381875367","15263548166808880668931526071918599213486304986568041225530763198080863123460"],["1","0"]],"pi_c":["19436657118428907159797452603147484224001485778247802421550746896397384983856","3196080941424733966474612046643629155603406339625206953551307106366932306579","1"],"protocol":"groth16","curve":"bn128"},"boundConfig":{"circuitIdentifier":"proto-pod-gpc_1o-12e-5md-4nv-0ei-1x5l-0x0t-0ov3-1ov4","pods":{"security_badge":{"entries":{"expiry_date":{"isRevealed":false,"inRange":{"min":1746835200000,"max":1902009600000}},"holder_smart_character_address":{"isRevealed":true},"issued_date":{"isRevealed":false,"inRange":{"min":0,"max":1746835200000}},"pod_type":{"isRevealed":true},"security_level":{"isRevealed":false,"inRange":{"min":3,"max":10}}}}}},"revealedClaims":{"pods":{"security_badge":{"entries":{"holder_smart_character_address":"0x6d11ac8f376b6284a7e5d62a340f71869b3063ae","pod_type":"corpName.security_badge"},"signerPublicKey":"3iREOe5OdCEZ0KaF4pOfFc5nMvG6iZbY7GeaMy2P3xw"}}}}'

    //Parse the Proof Data to JSON
    const receivedFromProver = JSON.parse(proofMessage);

    //Get the Proof, Bound Config, and Revealed Claims
    const proof = receivedFromProver.proof;
    const boundConfig = boundConfigFromJSON(receivedFromProver.boundConfig);
    const revealedClaims = revealedClaimsFromJSON(receivedFromProver.revealedClaims);

    //Verify the Proof
    async function VerifyProof(){
        const verifyConfig: GPCBoundConfig = {
            ...expectedProofConfig,
            circuitIdentifier: boundConfig.circuitIdentifier
        }

        //Verify the Proof
        const isValid = await gpcVerify(
            proof,
            verifyConfig,
            revealedClaims,
            GPC_ARTIFACTS_PATH
        );

        //Print the result
        if(!isValid){
            throw new Error("Proof is invalid");   
        }

        console.log("Proof is valid");

        const officialPublicKey = "3iREOe5OdCEZ0KaF4pOfFc5nMvG6iZbY7GeaMy2P3xw"

        if(revealedClaims.pods.security_badge.signerPublicKey != officialPublicKey){
            throw new Error("Not the official signer");
        }

        const badgeEntries = revealedClaims.pods.security_badge.entries;

        const officialPodType = "corpName.security_badge"

        if(badgeEntries.pod_type.value !== officialPodType){
            throw new Error("Not the right type of POD");
        }

        console.log("Correct POD Type")

        const expectedHolderAddress = "0x6d11ac8f376b6284a7e5d62a340f71869b3063ae"

        const holderAddress = badgeEntries.holder_smart_character_address.value;

        if(holderAddress != expectedHolderAddress){
            throw new Error("Not the right holder address");
        }
            
        console.log("Verfied security badge for character", holderAddress);

        //Exit the program
        process.exit(0);
    }

    //Run the verification
    VerifyProof()
    ```
</CollapseCode>

### Step 7: Extract Data and Verify POD Type

Next, extract the revealed entries from the proof.

<CollapseCode>
    ``` javascript copy showLineNumbers filename="zkVerify.ts" {83}
    //Import the GPC Packages
    import {
        GPCProofConfig, gpcVerify,
        boundConfigFromJSON, revealedClaimsFromJSON,
        GPCBoundConfig
    } from "@pcd/gpc";

    //Import the GPC Artifacts
    const GPC_ARTIFACTS_PATH = "./node_modules/@pcd/proto-pod-gpc-artifacts";

    //Import the Proof Config
    const expectedProofConfig: GPCProofConfig = {
        pods: {
            security_badge: {
                entries: {
                    security_level: { 
                        isRevealed: false,
                        inRange: {
                            min: 3n,
                            max: 10n
                        }
                    },
                    holder_smart_character_address: { isRevealed: true },
                    issued_date: {
                        isRevealed: false,
                        inRange: {
                            min: 0n,
                            max: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime())
                        }
                    },
                    expiry_date: {
                        isRevealed: false,
                        inRange: {
                            min: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime()),
                            max: BigInt(new Date("2030-04-10T00:00:00.000Z").getTime())
                        }
                    },
                    pod_type: { isRevealed: true }
                }
            }
        }
    };

    //Import the Proof Data
    const proofMessage = '{"proof":{"pi_a":["18382096760714031908670127575213136578083295521215433334251317660564033378720","21030477399437073370476149069090721170053337449725832469428119547769133404031","1"],"pi_b":[["3441945812976152446306624803438621070022957041244859610613891371824225138447","13484259047285811435888513023900289692797242279920846387677844428076348775051"],["5980242761206523890540011436691111069270009222756334744394997945384381875367","15263548166808880668931526071918599213486304986568041225530763198080863123460"],["1","0"]],"pi_c":["19436657118428907159797452603147484224001485778247802421550746896397384983856","3196080941424733966474612046643629155603406339625206953551307106366932306579","1"],"protocol":"groth16","curve":"bn128"},"boundConfig":{"circuitIdentifier":"proto-pod-gpc_1o-12e-5md-4nv-0ei-1x5l-0x0t-0ov3-1ov4","pods":{"security_badge":{"entries":{"expiry_date":{"isRevealed":false,"inRange":{"min":1746835200000,"max":1902009600000}},"holder_smart_character_address":{"isRevealed":true},"issued_date":{"isRevealed":false,"inRange":{"min":0,"max":1746835200000}},"pod_type":{"isRevealed":true},"security_level":{"isRevealed":false,"inRange":{"min":3,"max":10}}}}}},"revealedClaims":{"pods":{"security_badge":{"entries":{"holder_smart_character_address":"0x6d11ac8f376b6284a7e5d62a340f71869b3063ae","pod_type":"corpName.security_badge"},"signerPublicKey":"3iREOe5OdCEZ0KaF4pOfFc5nMvG6iZbY7GeaMy2P3xw"}}}}'

    //Parse the Proof Data to JSON
    const receivedFromProver = JSON.parse(proofMessage);

    //Get the Proof, Bound Config, and Revealed Claims
    const proof = receivedFromProver.proof;
    const boundConfig = boundConfigFromJSON(receivedFromProver.boundConfig);
    const revealedClaims = revealedClaimsFromJSON(receivedFromProver.revealedClaims);

    //Verify the Proof
    async function VerifyProof(){
        const verifyConfig: GPCBoundConfig = {
            ...expectedProofConfig,
            circuitIdentifier: boundConfig.circuitIdentifier
        }

        //Verify the Proof
        const isValid = await gpcVerify(
            proof,
            verifyConfig,
            revealedClaims,
            GPC_ARTIFACTS_PATH
        );

        //Print the result
        if(!isValid){
            throw new Error("Proof is invalid");   
        }

        console.log("Proof is valid");

        const officialPublicKey = "3iREOe5OdCEZ0KaF4pOfFc5nMvG6iZbY7GeaMy2P3xw"

        if(revealedClaims.pods.security_badge.signerPublicKey != officialPublicKey){
            throw new Error("Not the official signer");
        }

        const badgeEntries = revealedClaims.pods.security_badge.entries;

        const officialPodType = "corpName.security_badge"

        if(badgeEntries.pod_type.value !== officialPodType){
            throw new Error("Not the right type of POD");
        }

        console.log("Correct POD Type")

        const expectedHolderAddress = "0x6d11ac8f376b6284a7e5d62a340f71869b3063ae"

        const holderAddress = badgeEntries.holder_smart_character_address.value;

        if(holderAddress != expectedHolderAddress){
            throw new Error("Not the right holder address");
        }
            
        console.log("Verfied security badge for character", holderAddress);

        //Exit the program
        process.exit(0);
    }

    //Run the verification
    VerifyProof()
    ```
</CollapseCode>

Then, verify it is the correct POD Type. 

<CollapseCode>
    ``` javascript copy showLineNumbers filename="zkVerify.ts" {87-91}
    //Import the GPC Packages
    import {
        GPCProofConfig, gpcVerify,
        boundConfigFromJSON, revealedClaimsFromJSON,
        GPCBoundConfig
    } from "@pcd/gpc";

    //Import the GPC Artifacts
    const GPC_ARTIFACTS_PATH = "./node_modules/@pcd/proto-pod-gpc-artifacts";

    //Import the Proof Config
    const expectedProofConfig: GPCProofConfig = {
        pods: {
            security_badge: {
                entries: {
                    security_level: { 
                        isRevealed: false,
                        inRange: {
                            min: 3n,
                            max: 10n
                        }
                    },
                    holder_smart_character_address: { isRevealed: true },
                    issued_date: {
                        isRevealed: false,
                        inRange: {
                            min: 0n,
                            max: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime())
                        }
                    },
                    expiry_date: {
                        isRevealed: false,
                        inRange: {
                            min: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime()),
                            max: BigInt(new Date("2030-04-10T00:00:00.000Z").getTime())
                        }
                    },
                    pod_type: { isRevealed: true }
                }
            }
        }
    };

    //Import the Proof Data
    const proofMessage = '{"proof":{"pi_a":["18382096760714031908670127575213136578083295521215433334251317660564033378720","21030477399437073370476149069090721170053337449725832469428119547769133404031","1"],"pi_b":[["3441945812976152446306624803438621070022957041244859610613891371824225138447","13484259047285811435888513023900289692797242279920846387677844428076348775051"],["5980242761206523890540011436691111069270009222756334744394997945384381875367","15263548166808880668931526071918599213486304986568041225530763198080863123460"],["1","0"]],"pi_c":["19436657118428907159797452603147484224001485778247802421550746896397384983856","3196080941424733966474612046643629155603406339625206953551307106366932306579","1"],"protocol":"groth16","curve":"bn128"},"boundConfig":{"circuitIdentifier":"proto-pod-gpc_1o-12e-5md-4nv-0ei-1x5l-0x0t-0ov3-1ov4","pods":{"security_badge":{"entries":{"expiry_date":{"isRevealed":false,"inRange":{"min":1746835200000,"max":1902009600000}},"holder_smart_character_address":{"isRevealed":true},"issued_date":{"isRevealed":false,"inRange":{"min":0,"max":1746835200000}},"pod_type":{"isRevealed":true},"security_level":{"isRevealed":false,"inRange":{"min":3,"max":10}}}}}},"revealedClaims":{"pods":{"security_badge":{"entries":{"holder_smart_character_address":"0x6d11ac8f376b6284a7e5d62a340f71869b3063ae","pod_type":"corpName.security_badge"},"signerPublicKey":"3iREOe5OdCEZ0KaF4pOfFc5nMvG6iZbY7GeaMy2P3xw"}}}}'

    //Parse the Proof Data to JSON
    const receivedFromProver = JSON.parse(proofMessage);

    //Get the Proof, Bound Config, and Revealed Claims
    const proof = receivedFromProver.proof;
    const boundConfig = boundConfigFromJSON(receivedFromProver.boundConfig);
    const revealedClaims = revealedClaimsFromJSON(receivedFromProver.revealedClaims);

    //Verify the Proof
    async function VerifyProof(){
        const verifyConfig: GPCBoundConfig = {
            ...expectedProofConfig,
            circuitIdentifier: boundConfig.circuitIdentifier
        }

        //Verify the Proof
        const isValid = await gpcVerify(
            proof,
            verifyConfig,
            revealedClaims,
            GPC_ARTIFACTS_PATH
        );

        //Print the result
        if(!isValid){
            throw new Error("Proof is invalid");   
        }

        console.log("Proof is valid");

        const officialPublicKey = "3iREOe5OdCEZ0KaF4pOfFc5nMvG6iZbY7GeaMy2P3xw"

        if(revealedClaims.pods.security_badge.signerPublicKey != officialPublicKey){
            throw new Error("Not the official signer");
        }

        const badgeEntries = revealedClaims.pods.security_badge.entries;

        const officialPodType = "corpName.security_badge"

        if(badgeEntries.pod_type.value !== officialPodType){
            throw new Error("Not the right type of POD");
        }

        console.log("Correct POD Type")

        const expectedHolderAddress = "0x6d11ac8f376b6284a7e5d62a340f71869b3063ae"

        const holderAddress = badgeEntries.holder_smart_character_address.value;

        if(holderAddress != expectedHolderAddress){
            throw new Error("Not the right holder address");
        }
            
        console.log("Verfied security badge for character", holderAddress);

        //Exit the program
        process.exit(0);
    }

    //Run the verification
    VerifyProof()
    ```
</CollapseCode>

### Step 8: Verify the Character Address

Next, verify the character address to make sure it's for the correct holder.

<CollapseCode>
    ``` javascript copy showLineNumbers filename="zkVerify.ts" {87-91}
    //Import the GPC Packages
    import {
        GPCProofConfig, gpcVerify,
        boundConfigFromJSON, revealedClaimsFromJSON,
        GPCBoundConfig
    } from "@pcd/gpc";

    //Import the GPC Artifacts
    const GPC_ARTIFACTS_PATH = "./node_modules/@pcd/proto-pod-gpc-artifacts";

    //Import the Proof Config
    const expectedProofConfig: GPCProofConfig = {
        pods: {
            security_badge: {
                entries: {
                    security_level: { 
                        isRevealed: false,
                        inRange: {
                            min: 3n,
                            max: 10n
                        }
                    },
                    holder_smart_character_address: { isRevealed: true },
                    issued_date: {
                        isRevealed: false,
                        inRange: {
                            min: 0n,
                            max: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime())
                        }
                    },
                    expiry_date: {
                        isRevealed: false,
                        inRange: {
                            min: BigInt(new Date("2025-05-10T00:00:00.000Z").getTime()),
                            max: BigInt(new Date("2030-04-10T00:00:00.000Z").getTime())
                        }
                    },
                    pod_type: { isRevealed: true }
                }
            }
        }
    };

    //Import the Proof Data
    const proofMessage = '{"proof":{"pi_a":["18382096760714031908670127575213136578083295521215433334251317660564033378720","21030477399437073370476149069090721170053337449725832469428119547769133404031","1"],"pi_b":[["3441945812976152446306624803438621070022957041244859610613891371824225138447","13484259047285811435888513023900289692797242279920846387677844428076348775051"],["5980242761206523890540011436691111069270009222756334744394997945384381875367","15263548166808880668931526071918599213486304986568041225530763198080863123460"],["1","0"]],"pi_c":["19436657118428907159797452603147484224001485778247802421550746896397384983856","3196080941424733966474612046643629155603406339625206953551307106366932306579","1"],"protocol":"groth16","curve":"bn128"},"boundConfig":{"circuitIdentifier":"proto-pod-gpc_1o-12e-5md-4nv-0ei-1x5l-0x0t-0ov3-1ov4","pods":{"security_badge":{"entries":{"expiry_date":{"isRevealed":false,"inRange":{"min":1746835200000,"max":1902009600000}},"holder_smart_character_address":{"isRevealed":true},"issued_date":{"isRevealed":false,"inRange":{"min":0,"max":1746835200000}},"pod_type":{"isRevealed":true},"security_level":{"isRevealed":false,"inRange":{"min":3,"max":10}}}}}},"revealedClaims":{"pods":{"security_badge":{"entries":{"holder_smart_character_address":"0x6d11ac8f376b6284a7e5d62a340f71869b3063ae","pod_type":"corpName.security_badge"},"signerPublicKey":"3iREOe5OdCEZ0KaF4pOfFc5nMvG6iZbY7GeaMy2P3xw"}}}}'

    //Parse the Proof Data to JSON
    const receivedFromProver = JSON.parse(proofMessage);

    //Get the Proof, Bound Config, and Revealed Claims
    const proof = receivedFromProver.proof;
    const boundConfig = boundConfigFromJSON(receivedFromProver.boundConfig);
    const revealedClaims = revealedClaimsFromJSON(receivedFromProver.revealedClaims);

    //Verify the Proof
    async function VerifyProof(){
        const verifyConfig: GPCBoundConfig = {
            ...expectedProofConfig,
            circuitIdentifier: boundConfig.circuitIdentifier
        }

        //Verify the Proof
        const isValid = await gpcVerify(
            proof,
            verifyConfig,
            revealedClaims,
            GPC_ARTIFACTS_PATH
        );

        //Print the result
        if(!isValid){
            throw new Error("Proof is invalid");   
        }

        console.log("Proof is valid");

        const officialPublicKey = "3iREOe5OdCEZ0KaF4pOfFc5nMvG6iZbY7GeaMy2P3xw"

        if(revealedClaims.pods.security_badge.signerPublicKey != officialPublicKey){
            throw new Error("Not the official signer");
        }

        const badgeEntries = revealedClaims.pods.security_badge.entries;

        const officialPodType = "corpName.security_badge"

        if(badgeEntries.pod_type.value !== officialPodType){
            throw new Error("Not the right type of POD");
        }

        console.log("Correct POD Type")

        const expectedHolderAddress = "0x6d11ac8f376b6284a7e5d62a340f71869b3063ae"

        const holderAddress = badgeEntries.holder_smart_character_address.value;

        if(holderAddress != expectedHolderAddress){
            throw new Error("Not the right holder address");
        }
            
        console.log("Verfied security badge for character", holderAddress);

        //Exit the program
        process.exit(0);
    }

    //Run the verification
    VerifyProof()
    ```
</CollapseCode>

### Step 9: Run the Code

You can now run the verification script using:

```bash copy copy
pnpm tsx verify.ts
```

</Steps>

## More Information

GPCs are also able to prove information about multiple PODs. For example, a proof can be generated using GPC that a number in POD A is larger than POD B without revealing the specific values.

This could be useful for example with having a single proof which proves that a player made a jump after the starting time and a jump to the end system at the time they say they ended the race. The proof configuration would look similar to:

```bash copy copy
{
  pods: {
    start: {
      entries: {
        originId: { isRevealed: true },
        shipId: { isRevealed: false, equalsEntry: "finish.shipId" },
        time: {
          isRevealed: false,
          inRange: { min: RACE_START_TIME, max: RACE_END_TIME }
        }
      }
    },
    finish: {
      entries: {
        destinationId: { isRevealed: true },
        shipId: { isRevealed: false },
        time: {
          isRevealed: true,
          inRange: { min: RACE_START_TIME, max: RACE_END_TIME },
          greaterThanEq: "start.time"
       }
      }
    }
  }
}
```

For more information, you can visit the official POD documentation for ZK Proofs at https://pod.org/gpc/introduction and https://pod.org/gpc/resources