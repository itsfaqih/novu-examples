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

<main class="page">
  <section class="shell" aria-labelledby="page-title">
    <div class="eyebrow">Novu Svelte example</div>
    <h1 id="page-title">Novu Svelte Inbox</h1>
    <p class="intro">Custom notification center example</p>

    <section class="demo-card" aria-label="Novu Inbox example">
      <header class="demo-header">
        <div>
          <h2>Notifications in your app</h2>
          <p>Open the notification button to view your latest notifications.</p>
        </div>

        {#if applicationIdentifier && subscriber}
          <NovuInbox
            applicationIdentifier={applicationIdentifier}
            subscriber={subscriber}
          />
        {:else}
          <div class="missing-config" role="alert">
            Set the Novu environment variables to enable the Inbox.
          </div>
        {/if}
      </header>

      <div class="demo-body">
        <p class="section-label">Inbox example</p>
        <h2>Stay up to date with your notifications</h2>
        <p>
          Svelte owns the component lifecycle while Novu handles notification data, real-time
          updates, and the Inbox interface.
        </p>
      </div>
    </section>
  </section>
</main>

<style>
  :global(*) {
    box-sizing: border-box;
  }

  :global(body) {
    margin: 0;
    color: #111827;
    background: #f5f5f5;
    font-family:
      Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  }

  .page {
    min-height: 100vh;
    padding: 4rem 1.5rem;
  }

  .shell {
    width: min(100%, 50rem);
    margin: 0 auto;
  }

  .eyebrow,
  .section-label {
    color: #6366f1;
    font-size: 0.75rem;
    font-weight: 700;
    letter-spacing: 0.12em;
    text-transform: uppercase;
  }

  h1,
  h2,
  p {
    margin: 0;
  }

  h1 {
    margin-top: 0.75rem;
    font-size: clamp(2.5rem, 8vw, 5rem);
    line-height: 0.95;
    letter-spacing: -0.06em;
  }

  .intro {
    margin-top: 1.25rem;
    color: #6b7280;
    font-size: 1.1rem;
  }

  .demo-card {
    margin-top: 3rem;
    overflow: hidden;
    border: 1px solid #e5e7eb;
    border-radius: 1.25rem;
    background: #ffffff;
    box-shadow: 0 1.5rem 4rem rgb(17 24 39 / 10%);
  }

  .demo-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 1.5rem;
    padding: 1.5rem;
    color: #ffffff;
    background: #6366f1;
  }

  .demo-header h2 {
    font-size: 1.25rem;
  }

  .demo-header p {
    margin-top: 0.35rem;
    color: #e0e7ff;
    font-size: 0.9rem;
  }

  .missing-config {
    max-width: 14rem;
    padding: 0.65rem 0.8rem;
    border: 1px solid rgb(255 255 255 / 35%);
    border-radius: 0.6rem;
    color: #eef2ff;
    font-size: 0.8rem;
    line-height: 1.4;
  }

  .demo-body {
    padding: 3rem 1.5rem 3.5rem;
    background: linear-gradient(135deg, #eef2ff, #ffffff 60%);
  }

  .demo-body h2 {
    max-width: 28rem;
    margin-top: 0.75rem;
    font-size: clamp(1.75rem, 5vw, 3rem);
    letter-spacing: -0.04em;
  }

  .demo-body p:last-child {
    max-width: 35rem;
    margin-top: 1rem;
    color: #4b5563;
    line-height: 1.7;
  }

  @media (max-width: 38rem) {
    .page {
      padding: 2.5rem 1rem;
    }

    .demo-header {
      align-items: flex-start;
      flex-direction: column;
    }
  }
</style>
