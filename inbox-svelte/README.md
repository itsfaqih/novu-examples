# Novu Svelte Inbox example

This example shows how to embed the Novu Inbox in a SvelteKit app with `@novu/js`.

## Setup

1. Install the dependencies.

   ```bash
   npm install
   ```

2. Copy `.env.example` to `.env`.

   ```bash
   cp .env.example .env
   ```

3. Set `PUBLIC_NOVU_APPLICATION_IDENTIFIER` and `PUBLIC_NOVU_SUBSCRIBER_ID` in `.env`.

4. Start the development server.

   ```bash
   npm run dev
   ```

5. Open the local URL shown by Vite. Select the notification button to open the Inbox.

## What this example shows

- It loads `@novu/js` and `@novu/js/ui` in `onMount` so the browser-only UI does not run during SSR.
- It creates a `Novu` client and a `NovuUI` renderer with the shared application and subscriber options.
- It mounts the `Inbox` component into a Svelte element and unmounts it with the component lifecycle.
- It leaves notification data and real-time updates to Novu.

## Commands

```bash
npm run dev
npm run check
npm run build
npm run preview
```

