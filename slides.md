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

### I make open source things at [feathers.cloud](https://feathers.cloud)

<img src="/images/spacebird.svg" class="w-50 mx-a mt-4" alt="Space bird!" />

---
class: text-4xl
layout: cover
---

# What do we need to build a webapp?

<ul class="list-none">
  <li><img alt="Storage" src="/images/storage.svg" class="w-15 inline" /> Storage</li>
  <li><img alt="Database" src="/images/database.svg" class="w-15 inline" /> Database</li>
  <li><img alt="Compute" src="/images/compute.svg" class="w-15 inline" /> Compute</li>
  <li><img alt="Authentication" src="/images/authentication.svg" class="w-15 inline" /> Authentication</li>
</ul>

---
layout: cover
class: text-center
---

# The browser is a _universal_ application platform

<img alt="Universal" src="/images/universal.svg" class="mx-a w-70" />

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

Can be accessed via [IPFS]() as:

[ipfs://QmU3JRrRWESWX3ywNVx1A5zkSgpnwztniPhjNHG42yrtsG](https://ipfs.io/ipfs/QmU3JRrRWESWX3ywNVx1A5zkSgpnwztniPhjNHG42yrtsG)


---
class: text-center bg-gray-300
---

![IPFS network structure](/images/http-vs-ipfs.png)

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

# Compute

<img alt="Compute" src="/images/compute.svg" class="mx-a w-70" />

---
class: bg-gray-300 text-xl
---

<h1 style="color: #343979;">Webassembly</h1>

<p style="color: #343979;">A binary instruction format for high-performance web applications in any language.</p>

<img src="/images/webassembly.png" alt="Webassembly image" />

---
layout: cover
class: text-center
---

# Authentication

<img alt="Authentication" src="/images/authentication.svg" class="mx-a w-70" />

---
class: text-xl
---

# UCAN

User Controlled Authorization Networks (UCANs) enable a scalable and secure way of authorizing offline-first apps and distributed systems.

<img src="/images/ucan.png" class="w-8/12 mx-a mt-8" />

---
layout: cover
background: /images/passkey.png
class: text-xl
---

# Passkey

## A digital credential for passwordless authentication stored securely in the operating system, browser or hardware.

*Supported by over 99% of devices.*

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
- Secure by default
- Scalable
- Actually serverless

---
class: text-2xl
layout: two-cols
---

# Thank you!

- [feathers.cloud](https://feathers.cloud)
- [feathersjs.com](https://feathersjs.com)
- [fission.codes](https://fission.codes)
- [IPFS](https://ipfs.tech)
- [CRDT.tech](https://crdt.tech/)
- [Automerge](https://automerge.org/)
- [Passkey](https://www.passkeys.com/)
- [UCAN](https://ucan.xyz)
- [Rust WASM](https://www.rust-lang.org/what/wasm)

::right::

<QRCode page="1" size="400" />