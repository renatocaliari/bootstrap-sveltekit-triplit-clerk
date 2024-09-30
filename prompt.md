Create an application based on the criteria below.

Application purpose: [describe here]

Business rules:
- rule 1
- rule 2
- rule 3
  
Stack:
- Svelte 5 (Runes)
- SvelteKit
- shadcn-svelte (importante ser components para svelte)
- tailwind css (para o shadcn-svelte funcionar)
- Triplitdev (banco de dados frontend)
- Clerk Auth: configurar pra funcionar com ao Triplit.dev e SvelteKit.

Detalhes de config stack:
- Triplit should be set to local development.
- "AUTH" should be set to true or false in .env file. The app checks Auth variable ONLY in Local Development, so if Auth=false, the app ignores auth and allow use everything without login or permission. If in production mode, everything needs auth.
- the .env file must contain these variables:
  - # Triplit CLI Variables
  - TRIPLIT_DB_URL=http://localhost:6543
  - TRIPLIT_ANON_TOKEN=<anon token>
  - TRIPLIT_SERVICE_TOKEN=<service token>

  - # App variables
  - NEXT_PUBLIC_TRIPLIT_SERVER_URL=http://localhost:6543   
  - NEXT_PUBLIC_TRIPLIT_TOKEN=<anon token>
  - TRIPLIT_EXTERNAL_JWT_SECRET=<jwt secret>

- Triplit client needs to send the JWT token issued by Clerk with each request using TriplitClient.updateToken
- Triplit client should has this config: autoConnect: browser
- To allow Vite to bundle the files in the triplit directory created with triplit init, you can add the following configuration to vite.config.ts file in defineConfig: server: { fs: { allow: ['./triplit'] } }
- Create a README.md file with these info (or add these info if it already exists): Clerk -> Email, Phone, Username: disable password and enable "Email verification code"
- Follow the instructions in the code examples below.
  
<CODE EXAMPLE Triplit schema.ts>
import { Schema as S, type ClientSchema, type Entity, type Roles } from '@triplit/client';

// This is your schema definition.
//
// For all of the supported types and options, check the documentation:
//   https://triplit.com/docs/schemas/types
//
// Whenever you change your schema while the sync server is running
// you'll need to run
//
//   `triplit schema push`
//
// Read more about schema management:
//  https://www.triplit.dev/docs/schemas/updating

export const roles: Roles = {
	admin: {
		match: {
			role: 'admin',
		},
	},
	org_admin: {
		match: {
			sub: '$userId',
			org_id: '$orgId',
			org_role: 'org:admin',
		},
	},
	org_member: {
		match: {
			sub: '$userId',
			org_id: '$orgId',
			org_role: 'org:member',
		},
	},
	personal_user: {
		match: {
			sub: '$userId',
			org_id: undefined, // this is ignored by Triplit but here for clarity of what's coming from Clerk
			org_role: 'personal', // add { "org_role": "{{ org.role || personal }}" } to your Clerk JWT claims for this to match
		},
	},
};

export const schema = {
	todos: {
		schema: S.Schema({
			id: S.Id(),
			created_by_clerk_id: S.String({ nullable: true }),
			organization_id: S.String({ nullable: true }),
			text: S.String(),
			completed: S.Boolean({ default: false }),
			created_at: S.Date({ default: S.Default.now() }),
		}),
		permissions: {
			admin: {
				insert: { filter: [true] },
				read: { filter: [true] },
				update: { filter: [true] },
				delete: { filter: [true] },
			},
			org_member: {
				read: {
					filter: [['organization_id', '=', '$role.orgId']],
				},
				update: {
					filter: [['organization_id', '=', '$role.orgId']],
				},
				postUpdate: {
					filter: [['organization_id', '=', '$role.orgId']],
				},
			},
			org_admin: {
				insert: {
					filter: [
						['created_by_clerk_id', '=', '$role.userId'],
						['organization_id', '=', '$role.orgId'],
					],
				},
				read: {
					filter: [['organization_id', '=', '$role.orgId']],
				},
				update: {
					filter: [['organization_id', '=', '$role.orgId']],
				},
				postUpdate: {
					filter: [['organization_id', '=', '$role.orgId']],
				},
				delete: { filter: [['organization_id', '=', '$role.orgId']] },
			},
			personal_user: {
				insert: {
					filter: [
						['created_by_clerk_id', '=', '$role.userId'],
						['organization_id', '=', null],
					],
				},
				read: {
					filter: [
						['created_by_clerk_id', '=', '$role.userId'],
						['organization_id', '=', null],
					],
				},
				update: {
					filter: [
						['created_by_clerk_id', '=', '$role.userId'],
						['organization_id', '=', null],
					],
				},
				postUpdate: {
					filter: [
						['created_by_clerk_id', '=', '$role.userId'],
						['organization_id', '=', null],
					],
				},
				delete: {
					filter: [
						['created_by_clerk_id', '=', '$role.userId'],
						['organization_id', '=', null],
					],
				},
			},
		},
	},
} satisfies ClientSchema;

// Use the `Entity` type to extract clean types for your collections
export type Todo = Entity<typeof schema, 'todos'>;


</CODE EXAMPLE Triplit>

<CODE EXAMPLE +layout.server.ts>
// src/+layout.server.ts
import { buildClerkProps } from 'svelte-clerk/server';

// To enable Clerk SSR support, pass the `initialState` to the `ClerkProvider` component.
export const load = async ({ locals }) => {
	const token = await locals.auth?.getToken();
	return {
		...buildClerkProps(locals.auth),
		token,
	};
};
</CODE EXAMPLE layout>

<CODE EXAMPLE hooks.server.ts>
import { withClerkHandler } from 'svelte-clerk/server';

export const handle = withClerkHandler();
</CODE EXAMPLE hooks>

<CODE EXAMPLE lib/components/client.ts>
import { TriplitClient } from '@triplit/client';
import { schema } from '../../triplit/schema';
import { PUBLIC_TRIPLIT_SERVER_URL } from '$env/static/public';

// The TriplitClient has 4 main options
// - storage: The storage engine you want to use. This can be
//   'memory' or 'indexeddb'.
// - schema: The schema you defined for your app, which
//   will be used to generate types for client methods
//   and handle local database operations
// - serverUrl: The URL of your Triplit server
// - token: The token you got from the Triplit dashboard
//
// Without the serverUrl or token, the client will operate in
// offline mode

export const triplit = new TriplitClient({
	storage: 'memory',
	schema,
	serverUrl: PUBLIC_TRIPLIT_SERVER_URL,
	autoConnect: false,
});
</CODE EXAMPLE client.ts>
