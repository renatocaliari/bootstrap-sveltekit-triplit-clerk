https://www.triplit.dev/docs/frameworks/svelte

Svelte
======

👉

In anticipation of the release of Svelte 5, Triplit's Svelte bindings use [runes (opens in a new tab)](https://svelte.dev/blog/runes). You must be using one of the pre-release versions of Svelte 5. You can force the compiler into "runes mode" [like this (opens in a new tab)](https://svelte-5-preview.vercel.app/docs/runes#how-to-opt-in).

### New projects[](#new-projects)

The fast way to get started with Triplit is to use Create Triplit App which will scaffold a SvelteKit application with Triplit. Choose `Svelte` when prompted for the frontend framework.

npmpnpmyarnbun

    npm create triplit-app@latest my-app

### Existing projects[](#existing-projects)

If you have an existing Svelte or Svelte project, you can add the hooks provided by `@triplit/svelte`:

npmpnpmyarnbun

    npm i @triplit/svelte

SvelteKit[](#sveltekit)
-----------------------

If you are using SvelteKit, you can use the hooks described below, but you will need to ensure that the client only attempts to connect to the sync server over WebSockets when in the browser. You can do this by checking if the `browser` variable from `$app/environment` is `true`.

src/lib/client.ts

    import { TriplitClient } from '@triplit/client';
    import { browser } from '$app/environment';
    import { PUBLIC_TRIPLIT_URL, PUBLIC_TRIPLIT_TOKEN } from '$env/static/public';
     
    export const client = new TriplitClient({
      serverUrl: PUBLIC_TRIPLIT_URL,
      token: PUBLIC_TRIPLIT_TOKEN,
      autoConnect: browser,
    });

The suggested pattern is to create a client instance in a module and import it into your components.

### `vite.config.ts`[](#viteconfigts)

With the default SvelteKit configuration Vite will not be able to bundle files outside of the `src` or `node_modules` directory. To allow Vite to bundle the files in the `triplit` directory created with `triplit init`, you can add the following configuration to your `vite.config.ts` file:

vite.config.ts

    import { sveltekit } from '@sveltejs/kit/vite';
    import { defineConfig } from 'vite';
     
    export default defineConfig({
      plugins: [sveltekit()],
      server: { fs: { allow: ['./triplit'] } },
    });

useQuery[](#usequery)
---------------------

The `useQuery` hook subscribes to the provided query inside your Svelte component and will automatically unsubscribe from the query when the component unmounts.

The result of the hook is an object with the following properties:

*   `results`: An array of entities that satisfy the query.
*   `fetching`: A boolean that will be `true` initially, and then turn `false` when either the local fetch returns cached results or if there were no cached results and the remote fetch has completed.
*   `fetchingLocal`: A boolean indicating whether the query is currently fetching from the local cache.
*   `fetchingRemote`: A boolean indicating whether the query is currently fetching from the server.
*   `error`: An error object if the query failed to fetch.
*   `updateQuery`: A function that can be called to update the query.

App.svelte

    <script lang="ts">
      import { useQuery } from '@triplit/svelte'
      import { TriplitClient } from '@triplit/client'
      import { schema } from '../triplit/schema';
     
      const client = new TriplitClient({ schema });
     
      let data = useQuery(client, client.query('todos'));
    </script>
    <div>
      {#if data.fetching}
        <p>Loading...</p>
      {:else if data.error}
        <p>Error: {data.error.message}</p>
      {:else if data.results}
        <div>
          {#each data.results as todo}
            <div>{todo.text}</div>
          {/each}
        </div>
      {/if}
    </div>

useConnectionStatus[](#useconnectionstatus)
-------------------------------------------

The `useConnectionStatus` hook subscribes to changes to the connection status of the client and will automatically unsubscribe when the component unmounts.

App.svelte

    <script lang='ts'>
      import { useConnectionStatus } from '@triplit/svelte';
      import { TriplitClient } from '@triplit/client'
     
      const client = new TriplitClient({
        schema,
        serverUrl: PUBLIC_TRIPLIT_SERVER_URL,
        token: PUBLIC_TRIPLIT_TOKEN,
      });
     
      const connection = useConnectionStatus(client);
    </script>
    <div>
    {#if connection.status === 'OPEN'}
      <p>The client is connected</p>
    {:else}
      <p>The client is not connected</p>
    {/if}
    </div>

Last updated on September 20, 2024

[Angular](/docs/frameworks/angular "Angular")[Tanstack Router](/docs/frameworks/tanstack-router "Tanstack Router")

System

* * *

MIT 2024 © Nextra.
