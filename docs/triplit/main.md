Welcome to Triplit | Triplit Documentation

[Triplit](/docs)

CTRL K

[GitHubGitHub (opens in a new tab)](https://github.com/aspen-cloud/triplit)[DiscordDiscord (opens in a new tab)](https://discord.gg/q89sGWHqQ5)

CTRL K

*   [Welcome](/docs)
*   [Quick Start](/docs/quick-start)
*   [React + Vite tutorial](/docs/react-tutorial)
*   [FAQ](/docs/faq)
*   * * *
    
*   Client
    
    *   [Installation](/docs/client)
    *   `query`
        
        *   [Overview](/docs/client/query)
        *   [`select`](/docs/client/query/select)
        *   [`include`](/docs/client/query/include)
        *   [`where`](/docs/client/query/where)
        *   [`order`](/docs/client/query/order)
        *   [`limit`](/docs/client/query/limit)
        *   [`subquery`](/docs/client/query/subquery)
        *   [`syncStatus`](/docs/client/query/sync-status)
        *   [`vars`](/docs/client/query/variables)
        
    *   [`insert`](/docs/client/insert)
    *   [`update`](/docs/client/update)
    *   [`delete`](/docs/client/delete)
    *   [`transact`](/docs/client/transact)
    *   [`fetch`](/docs/client/fetch)
    *   [`fetchOne`](/docs/client/fetch-one)
    *   [`fetchById`](/docs/client/fetch-by-id)
    *   [`subscribe`](/docs/client/subscribe)
    *   [`subscribeWithPagination`](/docs/client/subscribe-with-pagination)
    *   [`subscribeWithExpand`](/docs/client/subscribe-with-expand)
    *   [Storage](/docs/client/storage)
    *   [Sync engine](/docs/client/sync-engine)
    *   [HTTP client](/docs/client/http-client)
    *   [Web Worker client](/docs/client/web-worker-client)
    *   [Options](/docs/client/options)
    *   [Debugging](/docs/client/debugging)
    
*   Schemas
    
    *   [Defining](/docs/schemas)
    *   [Updating](/docs/schemas/updating)
    *   [Types](/docs/schemas/types)
    *   [Relations](/docs/schemas/relations)
    *   [Permissions](/docs/schemas/permissions)
    *   [Rules](/docs/schemas/rules)
    
*   CLI
    
    *   [Installation](/docs/cli)
    *   [`clear`](/docs/cli/clear)
    *   [`dev`](/docs/cli/dev)
    *   [`init`](/docs/cli/init)
    *   [`repl`](/docs/cli/repl)
    *   [`roles`](/docs/cli/roles)
    *   [`schema`](/docs/cli/schema)
    *   [`seed`](/docs/cli/seed)
    
*   Frameworks
    
    *   [React](/docs/frameworks/react)
    *   [React Native](/docs/frameworks/react-native)
    *   [Angular](/docs/frameworks/angular)
    *   [Svelte](/docs/frameworks/svelte)
    *   [Tanstack Router](/docs/frameworks/tanstack-router)
    *   [Vue](/docs/frameworks/vue)
    
*   Triplit Cloud
    
    *   [Overview](/docs/triplit-cloud)
    *   [Managed machines](/docs/triplit-cloud/managed-machines)
    *   [Self-hosted deployments](/docs/triplit-cloud/self-hosted-deployments)
    *   [Triplit Console](/docs/triplit-cloud/triplit-console)
    
*   Authentication
    
    *   [Overview](/docs/auth)
    *   [Clerk](/docs/auth/clerk)
    *   [Supabase](/docs/auth/supabase)
    
*   * * *
    
*   [Self-hosting](/docs/self-hosting)
*   [Offline mode](/docs/offline-mode)
*   [Local development](/docs/local-development)
*   [Seeding a database](/docs/seeding)
*   [Server-side rendering](/docs/ssr)
*   [HTTP API](/docs/http-api)

System

On This Page

*   [Triplit is an open-source database that syncs data between server and browser in real-time:](#triplit-is-an-open-source-database-that-syncs-data-between-server-and-browser-in-real-time)
*   [Why Triplit?](#why-triplit)

[Edit this page on GitHub](https://github.com/aspen-cloud/triplit/tree/main/packages/docs/src/pages/index.mdx)

Welcome

Welcome to Triplit
==================

### Triplit is an open-source database that syncs data between server and browser in real-time:[](#triplit-is-an-open-source-database-that-syncs-data-between-server-and-browser-in-real-time)

🔄 **Real-time sync** with incremental updates and conflict resolution at the property level  
🏠 **Local caching** powered by a full-fledged client-side database  
💽 **Durable server-side storage** with an admin dashboard  
😃 **Optimistic updates** to make every interaction feel fast  
🔗 **Relational querying** for complex data models  
🛫 **Offline-mode** with automatic reconnection and consistency guarantees  
🔙 **Rollback and retry management** on failed updates  
🗂️ **Schemas** for data safety and Typescript autocompletion  
🔐 **Authorization** with row level granularity  
🤝 **Collaboration/Multiplayer** powered by [CRDTs (opens in a new tab)](https://en.wikipedia.org/wiki/Conflict-free_replicated_data_type)  
🏎️ **Low latency** with minimal network traffic using delta patches  
📝 **Simple API** for querying and mutating data in both vanilla Javascript and frameworks like React  

These features are required for any application that aims to be fast, responsive, and reliable, and are especially important for real-time or collaborative features.

Why Triplit?[](#why-triplit)
----------------------------

**Triplit is fast.** By default, every update using Triplit occurs optimistically and is saved in a local cache.

**Triplit is flexible.** Triplit supports arbitrary queries over it's cache and handles query invalidation for you. Triplit's cache also ensures your app will not sacrifice UX in poor or offline network conditions.

**Triplit is connected.** Triplit's syncing engine handles the complexity of propagating and merging updates, making it a great fit for quickly adding real-time or collaborative features to your application.

**Triplit is developer friendly.** Triplit takes care to be easy to use. It gets rid of the headaches of building a realtime system at all levels of the stack, and is potentially the only backend service you need for an app. Additionally, it is designed to work well with Typescript applications, ensuring your type definitions remain up to date as your schema evolves.

**Triplit is open source.** Triplit may be self-hosted, so you can run it on your own infrastructure. Triplit also provides [Triplit Cloud](/docs/triplit-cloud) - a hosted service that manages upgrades, auto-scaling, and offers technical support.

The rest of the docs will explain how to use Triplit in your application.

Last updated on September 20, 2024

[Quick Start](/docs/quick-start "Quick Start")

System

* * *

MIT 2024 © Nextra.
