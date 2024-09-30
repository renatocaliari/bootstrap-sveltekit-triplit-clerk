https://www.triplit.dev/docs/local-development

Local Development | Triplit Documentation

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

*   [Install the CLI](#install-the-cli)
*   [Start Triplit services](#start-triplit-services)
*   [Additional environment variables](#additional-environment-variables)

[Edit this page on GitHub](https://github.com/aspen-cloud/triplit/tree/main/packages/docs/src/pages/local-development.mdx)

Local development

Local Development
=================

Although you can connect directly to a managed or self hosted instance of a remote Triplit Database, you can also run a local instance of Triplit Database for development purposes. This not only helps separate your development and production environments, but Triplit's local development toolkit provides a lot of help for deploying updates to the Database. For this reason, we recommend running a local instance of Triplit Database when building most projects.

Install the CLI[](#install-the-cli)
-----------------------------------

If you haven't already, install the Triplit CLI and initialize a project. You can find instructions for doing so [here](/docs/getting-started).

Start Triplit services[](#start-triplit-services)
-------------------------------------------------

To start Triplit services, run the following command in your project directory:

    triplit dev

By default this will start the following services:

*   Triplit Console, running at port 6542
*   Triplit Database server, running at port 6543

And prints a usable Service Token and Anonymous Token for connecting to the database.

Additional environment variables[](#additional-environment-variables)
---------------------------------------------------------------------

If your project has a `.env` file, you may set the following environment variables to configure the Triplit Database server:

💡

If you're using a framework like [Vite (opens in a new tab)](https://vitejs.dev/guide/) or [Next.js (opens in a new tab)](https://nextjs.org/docs) you should add additional environmental variables prepended with `VITE_` or `NEXT_PUBLIC_` respectively for the `DB_URL` and `ANONYMOUS_TOKEN`. For example, `TRIPLIT_DB_URL` would become `VITE_TRIPLIT_DB_URL` or `NEXT_PUBLIC_TRIPLIT_DB_URL`.

*   `TRIPLIT_SERVICE_TOKEN` - The Service Token to use for connecting to the database for CLI commands. If not set, you may use a flag in the CLI (which takes precedent) or the CLI will prompt you for a key.
*   `TRIPLIT_DB_URL` - The URL to use for connecting to the database. If not set, you may use a flag in the CLI (which takes precedent) or use the default URL for the local database.
*   `TRIPLIT_JWT_SECRET` - Overrides the JWT secret used when generating local api tokens.
*   `TRIPLIT_EXTERNAL_JWT_SECRET` - Overrides the JWT secret used when generating external api tokens.
*   `TRIPLIT_CLAIMS_PATH` - A `.` separated path to read Triplit claims on external api tokens. If not set, claims are assumed to be at the root of the token.

Last updated on September 20, 2024

[Offline mode](/docs/offline-mode "Offline mode")[Seeding a database](/docs/seeding "Seeding a database")

System

* * *

MIT 2024 © Nextra.
