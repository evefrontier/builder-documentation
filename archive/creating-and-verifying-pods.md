# Creating and Verifying PODs

import { Steps } from "nextra/components" import { Tabs, Tab } from "nextra/components"

import { Callout } from "nextra/components"; import { CollapseCode } from "../components/CollapseCode";

This guide will cover how to create custom PODs and verify any POD using TypeScript. We'll use an example of a corporation badge granting security clearance, which you might need to present to get access to corp facilities.

Documentation around PODs is currently a work in progress.

## Pre-Requisites

### Required

Ensure you setup your tools using the [Setting up your tools](https://github.com/evefrontier/builder-documentation/blob/Main/Tools/README.md) guide, as you will need to use some of them to create and verify PODs.

### Optional

We would suggest that you read the [Introduction to PODs](https://github.com/evefrontier/builder-documentation/blob/Main/pods/README.md) page first if you haven't already, as it explains what **PODs** are and the benefits of them.

## Code

You can either follow along with the code or use the [POD directory](https://github.com/projectawakening/builder-examples/tree/develop/pods) of the [Builder Examples](https://github.com/projectawakening/builder-examples) which already has the code. You can then run the code using the last step of each of the below sections.

```bash
cd builder-examples/pods
```

## Creating the Private and Public POD Signing Key

Issuing a POD requires signing it using a Private Key. There are several ways to generate a private key, one of them is to generate it in JavaScript.

#### Step 1: Setup your Project

Firstly, create a file called **privateKeyGen.js** and open it with your IDE / text editor.

#### Step 2: Import the Packages

Import the **randomBytes** function that is included in Node.JS for generating the key data. Also import the **deriveSignerPublicKey** package from the POD Library.

\`\`\` javascript copy showLineNumbers filename="privateKeyGen.js" {1-2} //Import Packages const { randomBytes } = require('crypto'); const { deriveSignerPublicKey } = require('@pcd/pod');

````
const key = randomBytes(32);

//Convert the key to a hex string
const privateSigningKey = key.toString('hex');
console.log("Generated Private Key:")
console.log(privateSigningKey);

//Output Signer Public Key
const publicSigningKey = deriveSignerPublicKey(privateSigningKey);
console.log("\nGenerated Signer Public Key:")
console.log(publicSigningKey)
```
````

#### Step 3: Generate the random data

Next, generate 32 bytes of random data using the **randomBytes** function.

\`\`\` javascript copy showLineNumbers filename="privateKeyGen.js" {5} //Import Packages const { randomBytes } = require('crypto'); const { deriveSignerPublicKey } = require('@pcd/pod');

````
const key = randomBytes(32);

//Convert the key to a hex string
const privateSigningKey = key.toString('hex');
console.log("Generated Private Key:")
console.log(privateSigningKey);

//Output Signer Public Key
const publicSigningKey = deriveSignerPublicKey(privateSigningKey);
console.log("\nGenerated Signer Public Key:")
console.log(publicSigningKey)
```
````

#### Step 4: Convert + Output the Private Signing Key

Then, convert it to Hex which is one of the allowed formats for POD private signing keys and output it to the console. POD Private keys can either be Hex or Base64.

\`\`\` javascript copy showLineNumbers filename="privateKeyGen.js" {7-10} //Import Packages const { randomBytes } = require('crypto'); const { deriveSignerPublicKey } = require('@pcd/pod');

````
const key = randomBytes(32);

//Convert the key to a hex string
const privateSigningKey = key.toString('hex');
console.log("Generated Private Key:")
console.log(privateSigningKey);

//Output Signer Public Key
const publicSigningKey = deriveSignerPublicKey(privateSigningKey);
console.log("\nGenerated Signer Public Key:")
console.log(publicSigningKey)
```
````

#### Step 5: Generate + Output the Public Signing Key

Then, generate the public key from the generated private key using the **deriveSignerPublicKey** function for later use.

If you are creating and distributing PODs, we would **strongly** recommend you share your public POD signing key so that others can verify the source of the PODs. This is so others cannot create counterfeit PODs.

\`\`\` javascript copy showLineNumbers filename="privateKeyGen.js" {12-15} //Import Packages const { randomBytes } = require('crypto'); const { deriveSignerPublicKey } = require('@pcd/pod');

````
const key = randomBytes(32);

//Convert the key to a hex string
const privateSigningKey = key.toString('hex');
console.log("Generated Private Key:")
console.log(privateSigningKey);

//Output Signer Public Key
const publicSigningKey = deriveSignerPublicKey(privateSigningKey);
console.log("\nGenerated Signer Public Key:")
console.log(publicSigningKey)
```
````

#### Step 5: Run the Code

Next, run the code with:

```bash
node privateKeyGen.js
```

You should then get a output that looks like:

```bash
Generated Private Key:
44e64ee0f13e64e0515fb9726946a775cebaf9aea14913ececbb477976e6d0db

Generated Signer Public Key:
4qdzO+eDgZoXRpVPngqNXMcuRoF+vP6APIJEkzBaB40
```

Then save the keys for later use. Ensure you do not share the private key, as if others have access to the key, they will be able to fake the POD signing process.

## Creating a POD

#### Step 1: Setup your Project

Firstly, in the directory you want to use install the POD library with:

```bash
pnpm i @pcd/pod
```

Then, install tsx to easily run the typescript with:

```bash
pnpm i tsx
```

Then, create a file called create.ts and open it with your preferred IDE.

#### Step 2: Import Packages

Import the packages required to create and verify PODs using the below line.

\`\`\` typescript copy showLineNumbers filename="create.ts" {2} //Import Packages import { POD, PODEntries, JSONPOD, PODValue, podValueFromJSON, deriveSignerPublicKey } from "@pcd/pod";

````
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

//Output Signer Public Key
const publicSigningKey = deriveSignerPublicKey(privateSigningKey);
console.log("\nSigner Public Key")
console.log(publicSigningKey + "\n")

//Create the POD
const myPOD = POD.sign(myEntries, privateSigningKey);

//Convert POD to JSON then String
const jsonPOD: JSONPOD = myPOD.toJSON();
const serializedPOD: string = JSON.stringify(jsonPOD);

//Output POD
console.log(jsonPOD)
console.log("\nStringified\n")
console.log(serializedPOD)
```
````

#### Step 3: Create the POD Data

Next, you create the data for the POD. PODs have entries which store the data with the format in TypeScript being:

```tsx
entryName: {
    type: "dataType",
    value: VALUE
}
```

You can then use these entries in a **PODEntries** type variable as the input data for a POD.

\`\`\` typescript copy showLineNumbers filename="create.ts" {5-23} //Import Packages import { POD, PODEntries, JSONPOD, PODValue, podValueFromJSON, deriveSignerPublicKey } from "@pcd/pod";

````
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

//Output Signer Public Key
const publicSigningKey = deriveSignerPublicKey(privateSigningKey);
console.log("\nSigner Public Key")
console.log(publicSigningKey + "\n")

//Create the POD
const myPOD = POD.sign(myEntries, privateSigningKey);

//Convert POD to JSON then String
const jsonPOD: JSONPOD = myPOD.toJSON();
const serializedPOD: string = JSON.stringify(jsonPOD);

//Output POD
console.log(jsonPOD)
console.log("\nStringified\n")
console.log(serializedPOD)
```
````

#### Step 4: Output Public Signing Key (Optional)

You can also output your public signing key. You can also get this from the [Creating the Private and Public Key](creating-and-verifying-pods.md#creating-the-private-and-public-pod-signing-key) step.

\`\`\` typescript copy showLineNumbers filename="create.ts" {26-31} //Import Packages import { POD, PODEntries, JSONPOD, PODValue, podValueFromJSON, deriveSignerPublicKey } from "@pcd/pod";

````
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

//Output Signer Public Key
const publicSigningKey = deriveSignerPublicKey(privateSigningKey);
console.log("\nSigner Public Key")
console.log(publicSigningKey + "\n")

//Create the POD
const myPOD = POD.sign(myEntries, privateSigningKey);

//Convert POD to JSON then String
const jsonPOD: JSONPOD = myPOD.toJSON();
const serializedPOD: string = JSON.stringify(jsonPOD);

//Output POD
console.log(jsonPOD)
console.log("\nStringified\n")
console.log(serializedPOD)
```
````

#### Step 5: Create and Sign the POD

Next, create the POD by signing the POD data with your private key. Use the private key that you generated earlier for the value of the **privateSigningKey** variable.

Do not share your Private Key. This private key is for demonstration and should not be used in a live app. \`\`\` typescript copy showLineNumbers filename="create.ts" {26,34} //Import Packages import { POD, PODEntries, JSONPOD, PODValue, podValueFromJSON, deriveSignerPublicKey } from "@pcd/pod";

````
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

//Output Signer Public Key
const publicSigningKey = deriveSignerPublicKey(privateSigningKey);
console.log("\nSigner Public Key")
console.log(publicSigningKey + "\n")

//Create the POD
const myPOD = POD.sign(myEntries, privateSigningKey);

//Convert POD to JSON then String
const jsonPOD: JSONPOD = myPOD.toJSON();
const serializedPOD: string = JSON.stringify(jsonPOD);

//Output POD
console.log(jsonPOD)
console.log("\nStringified\n")
console.log(serializedPOD)
```
````

#### Step 6: Export the POD

Next, convert the POD to JSON, and stringify for ease of use and transfer.

\`\`\` typescript copy showLineNumbers filename="create.ts" {36-43} //Import Packages import { POD, PODEntries, JSONPOD, PODValue, podValueFromJSON, deriveSignerPublicKey } from "@pcd/pod";

````
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

//Output Signer Public Key
const publicSigningKey = deriveSignerPublicKey(privateSigningKey);
console.log("\nSigner Public Key")
console.log(publicSigningKey + "\n")

//Create the POD
const myPOD = POD.sign(myEntries, privateSigningKey);

//Convert POD to JSON then String
const jsonPOD: JSONPOD = myPOD.toJSON();
const serializedPOD: string = JSON.stringify(jsonPOD);

//Output POD
console.log(jsonPOD)
console.log("\nStringified\n")
console.log(serializedPOD)
```
````

#### Step 7: Run the Code

You can now run the code using:

```bash
pnpm tsx create.ts
```

## Verifying a POD

#### Step 1: File Setup

Firstly, create a file named verify.ts in the same directory as create.ts and open it in your IDE of choice.

#### Step 2: Import Packages

Import the packages required to create and verify PODs using the below line.

\`\`\` typescript copy showLineNumbers filename="verify.ts" {2} //Import Packages import { POD, PODEntries, JSONPOD, PODValue, podValueFromJSON } from "@pcd/pod";

````
//Fetch the POD String
const serializedPOD = '{"entries":{"expiry_date":{"date":"2026-04-10T00:00:00.000Z"},"holder_smart_character_address":"0x6d11ac8f376b6284a7e5d62a340f71869b3063ae","issued_date":{"date":"2025-04-10T00:00:00.000Z"},"level":4,"pod_type":"corpName.access_badge"},"signature":"IQkqxOjjxbiJNHd2mfxOmLEFsWFluw+ZL93MnRGcXJoZWLdUc5Y9p/qgZ/gL72250U6XBVZnEIahn2M3leuBAg","signerPublicKey":"xDP3ppa3qjpSJO+zmTuvDM2eku7O4MKaP2yCCKnoHZ4"}'

//Create the POD from the String
const receivedPOD: POD = POD.fromJSON(JSON.parse(serializedPOD));

//Verify the POD
if(!receivedPOD.verifySignature()){
    throw new Error("Invalid POD");
}

console.log("Verified POD")

const officialPublicKey = "xDP3ppa3qjpSJO+zmTuvDM2eku7O4MKaP2yCCKnoHZ4" 

if(receivedPOD.signerPublicKey != officialPublicKey){
    throw new Error("Not the official signer");
}

console.log("Verified Official Signer")

const officialPodType = "corpName.security_badge"

const podType = receivedPOD.content.getValue("pod_type")?.value;

if(podType != officialPodType){
    throw new Error("Not the official pod type");
}

console.log("Verified Official Pod Type")

//Get a value from the POD
const level = receivedPOD.content.getValue("security_level");

console.log("Level:", level?.value)
```
````

#### Step 3: Import the POD

Next, import the POD. You can get the stringified JSON from the creation step, and then convert it into a POD using the **POD.fromJSON** function.

\`\`\` typescript copy showLineNumbers filename="verify.ts" {4-8} //Import Packages import { POD, PODEntries, JSONPOD, PODValue, podValueFromJSON } from "@pcd/pod";

````
//Fetch the POD String
const serializedPOD = '{"entries":{"expiry_date":{"date":"2026-04-10T00:00:00.000Z"},"holder_smart_character_address":"0x6d11ac8f376b6284a7e5d62a340f71869b3063ae","issued_date":{"date":"2025-04-10T00:00:00.000Z"},"level":4,"pod_type":"corpName.access_badge"},"signature":"IQkqxOjjxbiJNHd2mfxOmLEFsWFluw+ZL93MnRGcXJoZWLdUc5Y9p/qgZ/gL72250U6XBVZnEIahn2M3leuBAg","signerPublicKey":"xDP3ppa3qjpSJO+zmTuvDM2eku7O4MKaP2yCCKnoHZ4"}'

//Create the POD from the String
const receivedPOD: POD = POD.fromJSON(JSON.parse(serializedPOD));

//Verify the POD
if(!receivedPOD.verifySignature()){
    throw new Error("Invalid POD");
}

console.log("Verified POD")

const officialPublicKey = "xDP3ppa3qjpSJO+zmTuvDM2eku7O4MKaP2yCCKnoHZ4" 

if(receivedPOD.signerPublicKey != officialPublicKey){
    throw new Error("Not the official signer");
}

console.log("Verified Official Signer")

const officialPodType = "corpName.security_badge"

const podType = receivedPOD.content.getValue("pod_type")?.value;

if(podType != officialPodType){
    throw new Error("Not the official pod type");
}

console.log("Verified Official Pod Type")

//Get a value from the POD
const level = receivedPOD.content.getValue("security_level");

console.log("Level:", level?.value)
```
````

#### Step 4: Verify POD

Next, verify the POD using the **.verifySignature()** function. This will check that none of the values have been tampered with.

\`\`\` typescript copy showLineNumbers filename="verify.ts" {10-15} //Import Packages import { POD, PODEntries, JSONPOD, PODValue, podValueFromJSON } from "@pcd/pod";

````
//Fetch the POD String
const serializedPOD = '{"entries":{"expiry_date":{"date":"2026-04-10T00:00:00.000Z"},"holder_smart_character_address":"0x6d11ac8f376b6284a7e5d62a340f71869b3063ae","issued_date":{"date":"2025-04-10T00:00:00.000Z"},"level":4,"pod_type":"corpName.access_badge"},"signature":"IQkqxOjjxbiJNHd2mfxOmLEFsWFluw+ZL93MnRGcXJoZWLdUc5Y9p/qgZ/gL72250U6XBVZnEIahn2M3leuBAg","signerPublicKey":"xDP3ppa3qjpSJO+zmTuvDM2eku7O4MKaP2yCCKnoHZ4"}'

//Create the POD from the String
const receivedPOD: POD = POD.fromJSON(JSON.parse(serializedPOD));

//Verify the POD
if(!receivedPOD.verifySignature()){
    throw new Error("Invalid POD");
}

console.log("Verified POD")

const officialPublicKey = "xDP3ppa3qjpSJO+zmTuvDM2eku7O4MKaP2yCCKnoHZ4" 

if(receivedPOD.signerPublicKey != officialPublicKey){
    throw new Error("Not the official signer");
}

console.log("Verified Official Signer")

const officialPodType = "corpName.security_badge"

const podType = receivedPOD.content.getValue("pod_type")?.value;

if(podType != officialPodType){
    throw new Error("Not the official pod type");
}

console.log("Verified Official Pod Type")

//Get a value from the POD
const level = receivedPOD.content.getValue("security_level");

console.log("Level:", level?.value)
```
````

#### Step 5: Verify Public Key

Next, verify the public key. Whilst it's not required, it's **strongly** recommended to ensure the POD was created from the correct source. Anyone can sign a POD, so what makes it trustworthy is knowing who signed it. Import your public signing key from the private and public key generation section _**(link to above section)**_.

If an issuer signs multiple types of PODs, an entry named `pod_type` is conventionally used to distinguish them. Verifiers can check this as well to ensure they don't misinterpret a POD's meaning.

\`\`\` typescript copy showLineNumbers filename="verify.ts" {17-23} //Import Packages import { POD, PODEntries, JSONPOD, PODValue, podValueFromJSON } from "@pcd/pod";

````
//Fetch the POD String
const serializedPOD = '{"entries":{"expiry_date":{"date":"2026-04-10T00:00:00.000Z"},"holder_smart_character_address":"0x6d11ac8f376b6284a7e5d62a340f71869b3063ae","issued_date":{"date":"2025-04-10T00:00:00.000Z"},"level":4,"pod_type":"corpName.access_badge"},"signature":"IQkqxOjjxbiJNHd2mfxOmLEFsWFluw+ZL93MnRGcXJoZWLdUc5Y9p/qgZ/gL72250U6XBVZnEIahn2M3leuBAg","signerPublicKey":"xDP3ppa3qjpSJO+zmTuvDM2eku7O4MKaP2yCCKnoHZ4"}'

//Create the POD from the String
const receivedPOD: POD = POD.fromJSON(JSON.parse(serializedPOD));

//Verify the POD
if(!receivedPOD.verifySignature()){
    throw new Error("Invalid POD");
}

console.log("Verified POD")

const officialPublicKey = "xDP3ppa3qjpSJO+zmTuvDM2eku7O4MKaP2yCCKnoHZ4" 

if(receivedPOD.signerPublicKey != officialPublicKey){
    throw new Error("Not the official signer");
}

console.log("Verified Official Signer")

const officialPodType = "corpName.security_badge"

const podType = receivedPOD.content.getValue("pod_type")?.value;

if(podType != officialPodType){
    throw new Error("Not the official pod type");
}

console.log("Verified Official Pod Type")

//Get a value from the POD
const level = receivedPOD.content.getValue("security_level");

console.log("Level:", level?.value)
```
````

#### Step 6: Verify POD Type

Next, verify the POD is the correct type. This is to ensure that you have verified the correct POD.

\`\`\` typescript copy showLineNumbers filename="verify.ts" {25-33} //Import Packages import { POD, PODEntries, JSONPOD, PODValue, podValueFromJSON } from "@pcd/pod";

````
//Fetch the POD String
const serializedPOD = '{"entries":{"expiry_date":{"date":"2026-04-10T00:00:00.000Z"},"holder_smart_character_address":"0x6d11ac8f376b6284a7e5d62a340f71869b3063ae","issued_date":{"date":"2025-04-10T00:00:00.000Z"},"level":4,"pod_type":"corpName.access_badge"},"signature":"IQkqxOjjxbiJNHd2mfxOmLEFsWFluw+ZL93MnRGcXJoZWLdUc5Y9p/qgZ/gL72250U6XBVZnEIahn2M3leuBAg","signerPublicKey":"xDP3ppa3qjpSJO+zmTuvDM2eku7O4MKaP2yCCKnoHZ4"}'

//Create the POD from the String
const receivedPOD: POD = POD.fromJSON(JSON.parse(serializedPOD));

//Verify the POD
if(!receivedPOD.verifySignature()){
    throw new Error("Invalid POD");
}

console.log("Verified POD")

const officialPublicKey = "xDP3ppa3qjpSJO+zmTuvDM2eku7O4MKaP2yCCKnoHZ4" 

if(receivedPOD.signerPublicKey != officialPublicKey){
    throw new Error("Not the official signer");
}

console.log("Verified Official Signer")

const officialPodType = "corpName.security_badge"

const podType = receivedPOD.content.getValue("pod_type")?.value;

if(podType != officialPodType){
    throw new Error("Not the official pod type");
}

console.log("Verified Official Pod Type")

//Get a value from the POD
const level = receivedPOD.content.getValue("security_level");

console.log("Level:", level?.value)
```
````

#### Step 7: Get POD Values (Optional)

You can also easily get the values from the POD using the below code.

\`\`\` typescript copy showLineNumbers filename="verify.ts" {35-38} //Import Packages import { POD, PODEntries, JSONPOD, PODValue, podValueFromJSON } from "@pcd/pod";

````
//Fetch the POD String
const serializedPOD = '{"entries":{"expiry_date":{"date":"2026-04-10T00:00:00.000Z"},"holder_smart_character_address":"0x6d11ac8f376b6284a7e5d62a340f71869b3063ae","issued_date":{"date":"2025-04-10T00:00:00.000Z"},"level":4,"pod_type":"corpName.access_badge"},"signature":"IQkqxOjjxbiJNHd2mfxOmLEFsWFluw+ZL93MnRGcXJoZWLdUc5Y9p/qgZ/gL72250U6XBVZnEIahn2M3leuBAg","signerPublicKey":"xDP3ppa3qjpSJO+zmTuvDM2eku7O4MKaP2yCCKnoHZ4"}'

//Create the POD from the String
const receivedPOD: POD = POD.fromJSON(JSON.parse(serializedPOD));

//Verify the POD
if(!receivedPOD.verifySignature()){
    throw new Error("Invalid POD");
}

console.log("Verified POD")

const officialPublicKey = "xDP3ppa3qjpSJO+zmTuvDM2eku7O4MKaP2yCCKnoHZ4" 

if(receivedPOD.signerPublicKey != officialPublicKey){
    throw new Error("Not the official signer");
}

console.log("Verified Official Signer")

const officialPodType = "corpName.security_badge"

const podType = receivedPOD.content.getValue("pod_type")?.value;

if(podType != officialPodType){
    throw new Error("Not the official pod type");
}

console.log("Verified Official Pod Type")

//Get a value from the POD
const level = receivedPOD.content.getValue("security_level");

console.log("Level:", level?.value)
```
````

#### Step 8: Run the Code

You can now run the verification script using:

```bash
pnpm tsx verify.ts
```

## Next Steps

If you want to learn how to create ZK Proofs to hide information in PODs, please visit [Generating and Verifying ZK Proofs from PODs](https://github.com/evefrontier/builder-documentation/blob/Main/zero-knowledge-proofs/README.md)

For more information, such as developer resources for PODs you can visit the official documentation for PODs here: https://pod.org/pod/resources
