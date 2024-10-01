# bootstrap-sveltekit-triplit-clerk
1. Use Claude dev to create the app using the prompt in this repository.
2. Download docs folder through https://download-directory.github.io/.
3. Add the unzipd folder docs to your new project folder.
4. Add the folder /docs to Claude Dev.
5. Create .env file in you project copying the content from .env.example. 
6. Install Triplit locally and run it
   - npm install --save-dev @triplit/cli
   - npx triplit dev --noSchema
7. Get the keys.
8. Go to clerk.com and creant an app.
   - Go to Api Keys: https://dashboard.clerk.com/apps/[app-id]/api-keys
   - Copy JWT public key and the other keys
