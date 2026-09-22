<script lang="ts">
  import { env } from '$env/dynamic/public';
  import NovuInbox from '$lib/NovuInbox.svelte';

  const applicationIdentifier = env.PUBLIC_NOVU_APPLICATION_IDENTIFIER;
  const subscriber = env.PUBLIC_NOVU_SUBSCRIBER_ID;
</script>

<svelte:head>
  <title>Novu Svelte Inbox</title>
  <meta
    name="description"
    content="A Svelte example that embeds the Novu Inbox with @novu/js."
  />
</svelte:head>

{#if !applicationIdentifier || !subscriber}
  <main class="page missing-page">
    <div class="center-container" role="alert">
      <h2>Missing Novu configuration</h2>
      <p>
        Please set PUBLIC_NOVU_APPLICATION_IDENTIFIER and PUBLIC_NOVU_SUBSCRIBER_ID in your .env
        file
      </p>
    </div>
  </main>
{:else}
  <main class="page">
    <section class="container" aria-labelledby="page-title">
      <header class="header">
        <h1 id="page-title">Novu Svelte Inbox</h1>
        <p>Custom notification center example</p>
      </header>

      <section class="content" aria-label="Notifications">
        <NovuInbox applicationIdentifier={applicationIdentifier} subscriber={subscriber} />
      </section>
    </section>
  </main>
{/if}

<style>
  :global(*) {
    box-sizing: border-box;
  }

  :global(body) {
    margin: 0;
    min-width: 20rem;
    min-height: 100vh;
    color: #111827;
    background: #f5f5f5;
    font-family:
      Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  }

  .page {
    min-height: 100vh;
  }

  .missing-page {
    display: grid;
    padding: 1.25rem;
    place-items: center;
  }

  .container {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    width: min(100%, 50rem);
    margin: 0 auto;
    background: #ffffff;
  }

  h1,
  h2,
  p {
    margin: 0;
  }

  .header {
    padding: 3.75rem 1.25rem 1.25rem;
    border-bottom-right-radius: 1.25rem;
    border-bottom-left-radius: 1.25rem;
    background: #6366f1;
  }

  .header h1 {
    color: #ffffff;
    font-size: 1.75rem;
    line-height: 1.2;
  }

  .header p {
    margin-top: 0.25rem;
    color: #e0e7ff;
    font-size: 0.875rem;
    line-height: 1.2;
  }

  .content {
    display: grid;
    flex: 1;
    min-height: 0;
    padding: 1.25rem;
    place-items: center;
  }

  .center-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 100%;
    padding: 1.25rem;
    text-align: center;
  }

  .center-container h2 {
    margin-bottom: 0.5rem;
    color: #ef4444;
    font-size: 1.125rem;
    font-weight: 600;
    line-height: 1.2;
  }

  .center-container p {
    color: #6b7280;
    font-size: 0.875rem;
    line-height: 1.2;
  }
</style>
