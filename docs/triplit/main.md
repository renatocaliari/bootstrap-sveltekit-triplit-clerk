https://www.triplit.dev/docs/quick-start

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
