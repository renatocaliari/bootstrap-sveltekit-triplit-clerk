Using Clerk with Triplit | Triplit Documentation


Authentication

Clerk

Using Clerk with Triplit
========================

[Clerk (opens in a new tab)](https://clerk.com) is a user authentication and management service that makes it easy to add user accounts to your application. It's super simple to get Clerk up and running with Triplit, and this guide will show you how.

💡

This guide assumes you have a Triplit project set up. If you don't have one, you can create one by following the [Quick Start](/docs/quick-start) guide.

### Create a Clerk account and application[](#create-a-clerk-account-and-application)

Follow the official [Clerk documentation (opens in a new tab)](https://docs.clerk.dev/) to create an account and application in their dashboard.

### Get your Clerk public key[](#get-your-clerk-public-key)

We also need to configure Triplit to validate the JWT tokens issued by Clerk. To do this, we need the RSA public key for your Clerk application. You can find this in the **API Keys** section of the Clerk dashboard.

![Clerk API Keys](/docs/_next/image?url=%2Fdocs%2F_next%2Fstatic%2Fmedia%2Fapi-keys-overview.b06e6436.png&w=3840&q=75)

Then click on the **Show JWT Public Key** button to reveal the public key.

![Clerk JWT Public Key](/docs/_next/image?url=%2Fdocs%2F_next%2Fstatic%2Fmedia%2Fapi-keys-public-key.ecf827f2.png&w=3840&q=75)

For local dev, add this to your `.env` file in your Triplit app, making sure to remove any newlines:

    TRIPLIT_EXTERNAL_JWT_SECRET=-----BEGIN PUBLIC KEY-----<some-encoded-stuff>-----END PUBLIC KEY-----

### Start the Triplit development server[](#start-the-triplit-development-server)

Now that we've configured Clerk and added the necessary environmental variables, we can start the Triplit development server:

    npx triplit dev

### Add Clerk to your Triplit app[](#add-clerk-to-your-triplit-app)

You can add Clerk to your Triplit app by installing the appropriate SDK. Clerk has official support for [Vanilla JavaScript (opens in a new tab)](https://clerk.com/docs/quickstarts/javascript), [React (opens in a new tab)](https://clerk.com/docs/quickstarts/react), [Next.js (opens in a new tab)](https://clerk.com/docs/quickstarts/nextjs), [Expo (opens in a new tab)](https://clerk.com/docs/quickstarts/expo), and more.

There are also community SDKs for frameworks like [Svelte (opens in a new tab)](https://github.com/markjaquith/clerk-sveltekit), [Vue (opens in a new tab)](https://vue-clerk.vercel.app/), and [Angular (opens in a new tab)](https://github.com/anagstef/ngx-clerk?tab=readme-ov-file#ngx-clerk). See the [Clerk documentation (opens in a new tab)](https://clerk.com/docs) for the full list of integrated frameworks.

### Add the Clerk token to your Triplit client[](#add-the-clerk-token-to-your-triplit-client)

Your Triplit client needs to send the JWT token issued by Clerk with each request. You can do this with the `updateToken` method for the `TriplitClient`.

    // get the Clerk client or session using your framework
    // e.g. `const auth = useAuth()` from @clerk/clerk-react
    auth.getToken().then(async (token) => {
      // If token has changed, update the client
      if (token !== client.token) {
        client.disconnect();
        client.updateToken(token);
        if (!client.token) {
          // If no token, we are unauthenticated, reset the client in case there is any data for the previous session stored
          await client.reset();
        } else {
          // If we have a token, connect to the server with new information
          client.connect();
        }
      }
    });

### Test locally[](#test-locally)

Run you Triplit app and sign in with Clerk. If you’re setup correctly, you should see the connection is established and your data is syncing with your server. If you can't connect, ensure that you set the `TRIPLIT_EXTERNAL_JWT_SECRET` environmental variables correctly.

### Configure your Triplit dashboard[](#configure-your-triplit-dashboard)

To use Clerk with a deployed Triplit server, you just need ensure that it can use the Clerk public key to verify incoming requests.

If you're using the Triplit Dashboard, you can add the public key to the **External JWT secret** input in your project settings.

![Triplit Project Settings](/docs/_next/image?url=%2Fdocs%2F_next%2Fstatic%2Fmedia%2Fauthentication.bb99cfba.png&w=3840&q=75)

If you're using a custom self-hosted server, you need to set the `EXTERNAL_JWT_SECRET` environmental variable to the public key.

### Add access control to your schema[](#add-access-control-to-your-schema)

Now that you have a user auth system set up, you can add roles and permissions to your Triplit schema. By default, the JWT issued by Clerk will set the `sub` claim to the authenticated user's unique identifier. In Triplit, you can use it to set up a simple `user` role:

    import { Roles } from '@triplit/client';
     
    export const roles: Roles = {
      user: {
        match: {
          sub: '$userId',
        },
      },
    };

[Read here to see how you can use this Role to define a permission.](/docs/docs/schemas/permissions).

Last updated on September 20, 2024

[Overview](/docs/auth "Overview")[Supabase](/docs/auth/supabase "Supabase")

System

* * *

MIT 2024 © Nextra.
