# solid-start-vanilla-extract

This repository is a reproduction.

<details>
  <summary>Table of contents</summary>

- [solid-start-vanilla-extract](#solid-start-vanilla-extract)
  - [About](#about)
    - [Description](#description)
    - [Conversations](#conversations)
      - [On Discord](#on-discord)
      - [On GitHub](#on-github)
  - [How to reproduce](#how-to-reproduce)
    - [Setup](#setup)
    - [Build](#build)
    - [Deploy](#deploy)
    - [Error](#error)
    - [Demonstration of resolving the issue](#demonstration-of-resolving-the-issue)
</details>



## About

### Description

Cannot deploy a [**Solid Start**](https://start.solidjs.com "start.solidjs.com") project to [**Vercel**](https://vercel.com "vercel.com") when using [**Vanilla Extract**](https://vanilla-extract.style "vanilla-extract.style").

### Conversations

An up-to-date list of conversations regarding this issue.


#### On Discord

- [**SolidJS**](https://discord.gg/solidjs "discord.gg/solidjs") > [**💬 Cannot build for Vercel**](https://discord.com/channels/722131463138705510/1265906604096753706 "#support > Cannot build for Vercel")

- [**Vanilla Extract**](https://discord.gg/6nCfPwwz6w) > [**#general > 💬**](https://discord.com/channels/885877446098964512/885877446891667529/1265938350402048011)

#### On GitHub

Awaiting

## How to reproduce

### Setup

1. Install [**NodeJS `20.x`**](https://nodejs.org "nodejs.org") ([Vercel requirement](https://vercel.link/node-version "vercel.link/node-version"))

2. Enable Corepack with `corepack enable`

### Build

1. Clone this repository

2. Install dependencies with `yarn`

3. Build with `yarn build`

### Deploy

> [!NOTE]
> You need a vercel account to deploy

Run `yarn vercel deploy --prebuilt`


### Error

If you open [**Runtime logs**](https://vercel.com/docs/observability/runtime-logs) of your deployment, you will see the following error:

```
Unhandled Rejection: TypeError: Cannot read properties of undefined (reading 'file')
    at Object.get (file:///var/task/chunks/runtime.mjs:5602:33)
    at file:///var/task/chunks/runtime.mjs:6385:32
    at process.processTicksAndRejections (node:internal/process/task_queues:95:5)
Node.js process exited with exit status: 128. The logs above can help with debugging the issue.
```

Here is a peek into `.vercel/output/functions/__nitro.func/chunks/runtime.mjs`
```js
/* ... */
output: {
  path: joinURL(
    app.config.server.baseURL ?? "",
    router.base,
    bundlerManifest[id].file,
    //                  ^^^^
    // at Object.get (runtime.mjs:5602:33)
  ),
},
/* ... */
```

### Demonstration of resolving the issue

1. Remove `plugins: [vanillaExtract()]` from `app.config.ts`.
2. Build and deploy again

It resolves the issue, verifying that Vanilla Extract is the cause (or Solid Start itself, or vinxi, or Nitro).
