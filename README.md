# Carlton's Utilities

## Creation History

This code repo is based of:

- https://github.com/carltonj2000/ccajgui-old

The following have been update:

- ShadCn-svelte UI Library
- Lucide-svelte Icon Library
- mode-watcher for dark/light mode
- Command Line Options - Testing presently

## Code History

The instructions below are summarized from:

- https://kit.svelte.dev/docs/creating-a-project
- https://tauri.app/v1/guides/getting-started/setup/sveltekit

```bash
npm create svelte@latest ccajgui
cd ccajgui/
npm install lucide-svelte
npm install -D prettier prettier-plugin-tailwindcss
npm install --save-dev @sveltejs/adapter-static
npm install --save-dev @tauri-apps/cli
npm install @tauri-apps/api
npm run tauri init
npm run tauri dev
npm run tauri build
# did not run the line below because I ran it previously and copied
npm run tauri -- icon src-tauri/icons/Paw0-1024.png

npm install mode-watcher

npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
npx shadcn-svelte@latest init
npx shadcn-svelte@latest add card
npx shadcn-svelte@latest add input
npx shadcn-svelte@latest add table
npx shadcn-svelte@latest add separator
npx shadcn-svelte@latest add select
```
