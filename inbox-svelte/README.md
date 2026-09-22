# Novu Svelte Inbox Example

This example uses [`@novu/js`](https://www.npmjs.com/package/@novu/js) to render a custom notification inbox in SvelteKit.

## Features

- Fetch notifications with `novu.notifications.list`
- Render a custom notification list
- Show loading, error, and empty states
- Receive real-time notification updates
- Configure a hosted or self-hosted Novu instance

## Prerequisites

- Node.js `^20.19.0`, `^22.12.0`, or `>=24.0.0`
- npm
- A Novu application identifier and subscriber ID

## Setup

1. Install the dependencies.

   ```bash
   npm install
   ```

2. Configure the environment variables.

   ```bash
   cp .env.example .env
   ```

   Set `PUBLIC_NOVU_APPLICATION_IDENTIFIER` and `PUBLIC_NOVU_SUBSCRIBER_ID` in `.env`. For a
   self-hosted Novu instance, also set `PUBLIC_NOVU_API_URL` and `PUBLIC_NOVU_SOCKET_URL`.

   Find the application identifier in the [Novu dashboard](https://web.novu.co/). Use the ID of the
   subscriber whose notifications you want to display.

3. Start the development server.

   ```bash
   npm run dev
   ```

4. Open the local URL shown by Vite. The page displays the notification list.

## Project Structure

```text
inbox-svelte/
├── src/
│   ├── lib/
│   │   └── NovuInbox.svelte      # Novu client and notification list
│   └── routes/
│       ├── +layout.svelte        # Sets the theme color
│       └── +page.svelte          # Reads configuration and renders the page
├── .env.example
├── package.json
├── package-lock.json
└── README.md
```

## How It Works

### 1. Create the Novu client

`NovuInbox.svelte` loads `@novu/js` inside `onMount`, so the browser-only client does not run during
server rendering. It creates a `Novu` client with the application identifier, subscriber ID, and
optional API and socket URLs.

### 2. Load and subscribe to notifications

The component subscribes to `notifications.list.updated` before it requests up to 40 notifications
with `novu.notifications.list`. If a real-time update arrives before the request resolves, the
component keeps the updated list instead of replacing it with the older response.

### 3. Render the notification states

The component displays loading, error, empty, or populated states. Each notification shows its
subject, body, and creation date.

## Key Files

- `src/lib/NovuInbox.svelte` creates and cleans up the Novu client, then renders notifications.
- `src/routes/+page.svelte` reads the public environment variables and renders the page.

## Learn More

- [Novu documentation](https://docs.novu.co/)
- [`@novu/js` on npm](https://www.npmjs.com/package/@novu/js)
- [SvelteKit documentation](https://svelte.dev/docs/kit/introduction)

## Commands

```bash
npm run dev
npm run check
npm run build
npm run preview
```
