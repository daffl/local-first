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

---
class: text-center
layout: cover
background: /images/moonbase.png
---

<h1 style="font-weight: 400;" class="m--40">Building local-first apps</h1>

---
class: text-center
layout: cover
---

# Hi, I'm David

### I do open source things at [feathers.cloud](https://feathers.cloud) and make music sometimes.

---
class: text-4xl
layout: cover
---

# What do we need to build a webapp?

- 💾 Storage
- 🗄️ Database
- 💻 Compute
- 🔐 Authentication

---
layout: cover
class: text-center
---

# The browser is a universal application platform

---
layout: cover
class: text-center
---

# 💾 Storage

---

## Content addressing

Gives _every_ file and directory a unique hash that can be used to look it up peer-to-peer from the closest phone/computer/server.

```html
<html>
  <body>
    <h1>Hello VanJS! 👋</h1>
  </body>
</html>
```

Can be accessed trough:

```
ipfs://fjdkslfdsjklf
```

---
class: text-center bg-gray-300
---

![IPFS network structure](/images/http-vs-ipfs.png)

---
layout: cover
class: text-center
---

# 🗄️ Database

---
class: text-xl
layout: image-right
image: /images/crdt-favicon.png
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

const repo = new Repo({
  network: [
    new BroadcastChannelNetworkAdapter(),
  ]
})

const handle = repo.create() 
// Or find an existing document
handle = repo.find('automerge:45NuQi1e45PKsemx8GhSCu62gyag')

handle.change(doc => {
  doc.messages.push({
    text: 'A new message'
  })
})

handle.on('change', ({doc}) => {
  // updated doc
})
```

---
layout: cover
class: text-center
---

# 💻 Compute

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

# 🔐 Authentication

---
layout: cover
background: /images/passkey.png
class: text-xl
---

# Passkey

## A digital credential for passwordless authentication stored securely in the operating system, browser or hardware.

*Supported by over 99% of devices.*

---

## Web crypto

---

# Why local first?

- Great developer experience
- Fast
- Works offline
- Easy to deploy
-
