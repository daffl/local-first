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
---

# Building local-first apps


---
class: text-center
layout: cover
---

# Hi, I'm David

### I do open source things at [feathers.cloud](https://feathers.cloud) and sometimes make music.

---
class: text-4xl
layout: cover
---

What do we need to build a webapp?

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
    <h1>Hello VanJS!</h1>
  </body>
</html>
```

```
ipfs://fjdkslfdsjklf
```

---

## Looking up files with IPFS

![IPFS network structure](/images/http-vs-ipfs.png)

---
layout: cover
class: text-center
---

# 🗄️ Database

---

## CRDTs

A Conflict-free replicated data type (CRDT) is a data structure that can be synchronised across devices.

- Works offline
- Can resolve conflicts automatically
- Instant local updates
- No server necessary

---

## Automerge

<CRDTDemo />

---
layout: cover
class: text-center
---

# 💻 Compute

---

## Webassembly

A binary instruction format for high-performance web applications in any language.

![Webassembly](/images/webassembly.png)

---
layout: cover
class: text-center
---

# 🔐 Authentication

---

## Web crypto

---

## Passkey

---

# Why local first?

- Great developer experience
- Fast
- Works offline
- Easy to deploy
-
