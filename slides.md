---
theme: ./theme
class: text-center
highlighter: shiki
lineNumbers: false
info: |
  ## Building local-first apps
  An overview of the emerging technologies
transition: slide-left
title: Building local-first apps
background: /images/moonbase.svg
---

<h1 style="font-weight: 400; color: #FED06C;" class="m--40 drop-shadow-lg">Building local-first apps</h1>

---
class: text-center
layout: cover
---

# Hi, I'm David

### I make open source things at
### [feathers.cloud](https://feathers.cloud) and [feathersjs.com](https://feathersjs.com)

<img src="/images/spacebird.svg" class="w-50 mx-a mt-4" alt="Space bird!" />

---
class: text-4xl
layout: cover
---

# What do we need to build a webapp?

<ul class="list-none">
  <li><img alt="Identity" src="/images/icons/security.svg" class="w-15 inline" /> Identity</li>
  <li><img alt="Database" src="/images/database.svg" class="w-15 inline" /> Database</li>
  <li><img alt="Storage" src="/images/storage.svg" class="w-15 inline" /> Storage</li>
  <li><img alt="Compute" src="/images/compute.svg" class="w-15 inline" /> Compute</li>
</ul>

---
layout: cover
class: text-center
---

# The browser is a _universal_ application platform

<img alt="Universal" src="/images/universal.svg" class="mx-a w-70" />

---
class: text-2xl
layout: image-right
image: /images/professor-bird.svg
---

# Why local first?

- Fast
- 100% uptime
- Works offline
- Easy to deploy
- Built in security
- Scalable
- Actually serverless

---
layout: cover
class: text-center
---

# Identity

<img alt="Identity" src="/images/icons/security.svg" class="mx-a w-70" />

---
class: text-xl
---

# Basic username and password

Built into HTTP, it sends the username and password as a Base64 encoded string.

```sh
GET /example HTTP/1.1
Host: www.example.com
Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=
```

Via the [fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) API:

```ts
const response = await fetch('/example', {
  headers: {
    Authorization: `Basic ${btoa('myusername:mypassword')}`
  }
})
```

---
class: text-xl
---

# API keys

A string identifying the sender usually passed in a custom (`X-`) header

```sh
GET /api/resource HTTP/1.1
Host: api.example.com
X-API-Key: abcdef12345
```

Via the [fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) API:

```ts
const response = await fetch('/example', {
  headers: {
    'X-API-Key': 'abcdef12345'
  }
})
```

---

# JSON Web Token (JWT)

Used to send claims (payload) between parties, in an HTTP header as `Bearer` authorization. It consists of Base64 encoded JSON data in the form of `HEADER.PAYLOAD.SIGNATURE`. `PAYLOAD` may include pre-defined values (like `iss`, `aud` and `eat`) and any other property.

```sh
GET /example HTTP/1.1
Host: www.example.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOm51bGx9.eyJpc3MiOiJEYXZpZCIsIm1lc3NhZ2UiOiJIaSJ9
```

In the browser:

```ts
const header = { typ: 'JWT', alg: null }
const payload = { iss: 'David', message: 'Hi' }

const jwt = btoa(JSON.stringify(header)) + '.' + btoa(JSON.stringify(payload))
const response = await fetch('/example', {
  headers: {
    Authorization: `Bearer ${jwt}`
  }
})
const signedJwt = jwt + '.' + calculateSignature(jwt, 'mysupersecret')
```

---

# Public key cryptography

```sh
# Generate private key
openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:2048
  -out private_key.pem.pem

# Generate public key
openssl rsa -pubout -in private_key.pem
  -out public_key.pem
```

We can do this now securely in the browser with the [Web Crypto API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API).

### public_key.pem

```
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAwGbSJdNKAg8Lk3f/P1JQ
Tox23cnApcEy8e/gBDC0dfz76aO8q+W0XpmltMIwLhol+B9uGPmW3iM7siCVnHox
ztr/uWvWGHt7mRn1wLzNRh8YxZS9IWqjVMDyp8RzCPdUbkZRSmfuvV/ZVOp1kBRe
182GiP3ng636bOTuLB538E/CSXQBRzxxKEwC9iXJcM1TFlbV3Pw+7w8Sd+1iR4RQ
uwmVjFsVO+1DmNAeNu/wjMeM+HoXGa+vYG/7L8zewgIv8R6vE6bqvhFbs1Y/ilEj
XMEANUJed7J+tUwxG8okPPHFivwbXcN4Far6Esd2SQCmwP8P9fDeC8fzXxx9deWP
XQIDAQAB
-----END PUBLIC KEY-----
```

---
layout: two-cols
---

# Decentralized Identifiers

DIDs are identifiers that represent a user-owned digital identity.

It is usually derived from the public key where the private key is stored securely on a device, browser or hardware key.

```ts
import { createClient } from '@featherscloud/auth'

const auth = createClient({
  appId: '<app-DID-here>'
})

// Get the unique DID for this device (browser)
console.log(await auth.getDeviceId())

'did:key:z6MkjTgqoet6Li8NbvpryRpcwW23yZdu6FAjuGDgTyNbePKa'
```

::right::

<img alt="Devices" src="/images/icons/devices.svg" />

---

# Self verifying JWTs

If we include the sender DID (public key) in the JWT payload and sign everything with our private key, we can verify the JWT without any additional knowledge.

```ts
import { createClient } from '@featherscloud/auth'

const auth = createClient({
  appId: '<app-DID-here>'
})
const did = await auth.getDeviceId()
const header = { typ: 'JWT', alg: 'ED25519' }
const payload = {
  iss: did,
  message: 'Hi'
}
const jwt = btoa(JSON.stringify(header)) + '.' + btoa(JSON.stringify(payload))

const signature = await auth.sign(new TextEncoder().encode(jwt))
const signedJwt = jwt + '.' + signature
```

---
class: text-xl
---

# UCAN

User Controlled Authorization Networks ([UCANs](https://ucan.xyz/)) enable a scalable and secure way of authorizing offline-first apps and distributed systems.

<img src="/images/ucan.png" class="w-8/12 mx-a mt-8" />

---
class: text-xl
---

<img alt="Feathers Cloud logo" class="invert w-1/2 mx-auto" src="/images/logo-cloud.svg" />

<h1 class="text-center my-5">Auth</h1>

Feathers Cloud Auth connects traditional passwordless login mechanisms like Email, Passkey, Social, SMS or MFA with decentralised identities to create local-first applications and serverless APIs with JavaScript and TypeScript.

- [app.feathers.cloud](https://app.feathers.cloud)
- [github.com/featherscloud/auth-starters](https://github.com/featherscloud/auth-starters)

---
layout: cover
background: /images/passkey.png
class: text-xl
---

# Passkey

## A digital credential for passwordless authentication stored securely in the operating system, browser or hardware.

*Supported by over 99% of devices.*

---
layout: cover
class: text-center
---

# Database

<img alt="Database" src="/images/database.svg" class="mx-a w-70" />

---
class: text-xl
layout: image-right
image: /images/sync-birds.svg
---

# CRDTs

A Conflict-free replicated data type (CRDT) is a data structure that can be synchronised across devices.[^1]

- Works offline
- Resolves conflicts automatically
- Instant local updates
- No server necessary

<br>
<br>

[^1]: [An Interactive Intro to CRDTs](https://jakelazaroff.com/words/an-interactive-intro-to-crdts/)

---

# Automerge

```ts
import { Repo } from '@automerge/automerge-repo'
import { BroadcastChannelNetworkAdapter } from '@automerge/automerge-repo-network-broadcastchannel'
import { IndexedDBStorageAdapter } from "@automerge/automerge-repo-storage-indexeddb"

const repo = new Repo({
  network: [
    new BroadcastChannelNetworkAdapter(),
  ],
  storage: new IndexedDBStorageAdapter()
})

const handle = repo.create() 
// Or find an existing document
handle = repo.find('automerge:45NuQi1e45PKsemx8GhSCu62gyag')

handle.change(doc => {
  doc.messages.push({
    text: 'A new message'
  })
})

handle.on('change', ({doc}) => {})
```

---

<QRCode page="12" />

---

<CRDTDemo />

---
layout: cover
class: text-center
---

# Storage

<img alt="Storage" src="/images/storage.svg" class="mx-a w-70" />

---
class: text-xl
---

# Content addressing

Gives _every_ file and directory a unique hash that can be used to look it up peer-to-peer from the closest phone/computer/server.


```html
<html>
  <body>
    <h1>Hello VanJS! 👋</h1>
  </body>
</html>
```

Can be accessed via [IPFS](https://www.ipfs.tech/) as:

[ipfs://QmU3JRrRWESWX3ywNVx1A5zkSgpnwztniPhjNHG42yrtsG](https://cf-ipfs.com/ipfs/QmU3JRrRWESWX3ywNVx1A5zkSgpnwztniPhjNHG42yrtsG)


---
class: text-center bg-gray-300
---

![IPFS network structure](/images/http-vs-ipfs.png)

# Compute

<img alt="Compute" src="/images/compute.svg" class="mx-a w-70" />

---
class: bg-gray-300 text-xl
---

<h1 style="color: #343979;">Webassembly</h1>

<p style="color: #343979;">A binary instruction format for high-performance web applications in any language.</p>

<img src="/images/webassembly.png" alt="Webassembly image" />

---
class: text-2xl
layout: two-cols
---

# Thank you!

- [app.feathers.cloud](https://app.feathers.cloud)
- [github.com/featherscloud/auth-starters](https://github.com/featherscloud/auth-starters)

- [feathersjs.com](https://feathersjs.com)
- [IPFS](https://ipfs.tech)
- [CRDT.tech](https://crdt.tech/)
- [Automerge](https://automerge.org/)
- [Passkey](https://www.passkeys.com/)
- [UCAN](https://ucan.xyz)
- [Rust WASM](https://www.rust-lang.org/what/wasm)

::right::

<QRCode page="1" size="400" />