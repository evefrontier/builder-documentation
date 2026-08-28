# Verifying a Sign-In (Server-Side)

Sign-in works by having the user sign a personal message with EVE Vault (via the wallet's `signPersonalMessage`) and verifying that signature on your server. This page shows the flow end to end: issue a nonce, have the wallet sign a message built from it, and verify the personal message signature server-side.

## Prerequisites

- [`@mysten/sui`](https://sdk.mystenlabs.com/typescript) installed on your server.
- A `SuiClient` for the network your users are on.
- Somewhere to store a pending nonce per session (cache, DB, signed cookie).

EVE Vault addresses are [zkLogin](https://docs.sui.io/concepts/cryptography/zklogin) addresses, so the verifier needs a `client` to check the zkLogin proof against the current epoch. An offline verify will not work.

## Steps

1. **Issue a nonce.** Generate a random, single-use, short-lived nonce. Store it against the session and send it to the client.

2. **Have the client sign it as a personal message.** The dApp builds the agreed message from the nonce and signs it with the wallet's `signPersonalMessage`, then posts back the resulting `signature` and the `address` it claims to be.

3. **Rebuild the message from the nonce you stored — do not use any message bytes from the request.** `signPersonalMessage` returns `{ bytes, signature }`; verifying against those returned `bytes` lets any message the wallet has ever signed be replayed with a fresh nonce.

4. **Verify** the signature against the rebuilt message, passing the claimed `address` so the check also confirms who signed it.

5. **Consume the nonce** once verification succeeds, so the same challenge cannot be used twice.

## Full example

```ts copy
import { verifyPersonalMessageSignature } from '@mysten/sui/verify'
import { SuiClient, getFullnodeUrl } from '@mysten/sui/client'

const client = new SuiClient({ url: getFullnodeUrl('mainnet') })

// Agree this format with your client and build it the SAME way on both sides.
function buildLoginMessage(nonce: string): Uint8Array {
  return new TextEncoder().encode(`Sign in to Example dApp\nnonce: ${nonce}`)
}

async function verifySignIn(req: { signature: string; address: string }) {
  // The nonce we issued and stored for this session — NOT anything from req.
  const storedNonce = await loadNonceForSession()
  if (!storedNonce) throw new Error('No pending sign-in')

  // Rebuild the message ourselves. We never touch req.bytes.
  const message = buildLoginMessage(storedNonce)

  try {
    // Throws if the signature is invalid for `message`, or if it wasn't
    // produced by `address`. `client` is required to check the zkLogin proof.
    await verifyPersonalMessageSignature(message, req.signature, {
      client,
      address: req.address,
    })
  } catch {
    throw new Error('Invalid sign-in')
  }

  await consumeNonce(storedNonce) // single-use: prevent replay
  return { address: req.address } // authenticated
}
```

The `address` option makes the verifier reject a signature that is valid but was produced by a different wallet, so a signature over your challenge from one account cannot be claimed by another.

## Checklist

- [ ] The nonce is random, single-use, and expires.
- [ ] The message is rebuilt on the server from the stored nonce.
- [ ] Client-supplied message bytes are never passed to the verifier.
- [ ] A `client` is passed so the zkLogin proof is verified against the current epoch.
- [ ] The signer is bound to the claimed address (via the `address` option or an explicit compare).
- [ ] The nonce is consumed once verification succeeds.
