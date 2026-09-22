# Novu Svelte Inbox example

This example shows how to use `@novu/js` in a SvelteKit app to render a custom notification list.

## Setup

1. Install the dependencies.

   ```bash
   npm install
   ```

2. Copy `.env.example` to `.env`.

   ```bash
   cp .env.example .env
   ```

3. Set `PUBLIC_NOVU_APPLICATION_IDENTIFIER` and `PUBLIC_NOVU_SUBSCRIBER_ID` in `.env`. For a
   self-hosted Novu instance, also set `PUBLIC_NOVU_API_URL` and `PUBLIC_NOVU_SOCKET_URL`.

4. Start the development server.

   ```bash
   npm run dev
   ```

5. Open the local URL shown by Vite. The page displays the notification list.

## What this example shows

- It loads `@novu/js` in `onMount` so the browser-only client does not run during SSR.
- It creates a `Novu` client with the application and subscriber options.
- It fetches notifications with `novu.notifications.list`.
- It renders loading, error, empty, and populated states.
- It updates the list when Novu emits `notifications.list.updated`.

## Commands

```bash
npm run dev
npm run check
npm run build
npm run preview
```
