<script lang="ts">
  import { onMount } from 'svelte';
  import type { Notification, Novu, NovuOptions } from '@novu/js';

  interface Props {
    applicationIdentifier: string;
    subscriber: string;
    apiUrl?: string;
    socketUrl?: string;
  }

  let { applicationIdentifier, subscriber, apiUrl, socketUrl }: Props = $props();

  type InboxState =
    | { status: 'loading' }
    | { status: 'ready'; notifications: Notification[] }
    | { status: 'error'; message: string };

  let inboxState: InboxState = $state.raw({ status: 'loading' });

  onMount(() => {
    const options = {
      applicationIdentifier,
      subscriber,
      ...(apiUrl ? { apiUrl } : {}),
      ...(socketUrl ? { socketUrl } : {})
    } satisfies NovuOptions;

    let disposed = false;
    let receivedRealtimeUpdate = false;
    let novu: Novu | undefined;
    let unsubscribe: (() => void) | undefined;

    const loadNotifications = async () => {
      const { Novu } = await import('@novu/js');

      if (disposed) return;

      novu = new Novu(options);
      unsubscribe = novu.on('notifications.list.updated', ({ data }) => {
        if (!disposed) {
          receivedRealtimeUpdate = true;
          inboxState = { status: 'ready', notifications: data.notifications };
        }
      });

      const response = await novu.notifications.list({ limit: 40 });

      if (disposed || receivedRealtimeUpdate) return;

      if (response.error) {
        inboxState = {
          status: 'error',
          message: response.error.message || String(response.error)
        };
        return;
      }

      inboxState = { status: 'ready', notifications: response.data?.notifications ?? [] };
    };

    void loadNotifications().catch((error: unknown) => {
      if (!disposed) {
        inboxState = {
          status: 'error',
          message: error instanceof Error ? error.message : String(error)
        };
      }
    });

    return () => {
      disposed = true;
      unsubscribe?.();
      void novu?.socket.disconnect();
    };
  });
</script>

{#if inboxState.status === 'loading'}
  <div class="state" role="status">
    <span class="spinner" aria-hidden="true"></span>
    <p class="loading-text">Loading notifications...</p>
  </div>
{:else if inboxState.status === 'error'}
  <div class="state" role="alert">
    <h2>Error loading notifications</h2>
    <p class="error-detail">{inboxState.message}</p>
  </div>
{:else if inboxState.notifications.length === 0}
  <div class="state" role="status">
    <p class="empty-text">No notifications yet</p>
  </div>
{:else}
  <ul class="notification-list" aria-label="Notifications">
    {#each inboxState.notifications as notification (notification.id)}
      <li class="notification-item">
        {#if notification.subject}
          <h2>{notification.subject}</h2>
        {/if}
        <p>{notification.body}</p>
        <time datetime={notification.createdAt}
          >{new Date(notification.createdAt).toLocaleDateString()}</time
        >
      </li>
    {/each}
  </ul>
{/if}

<style>
  :global(*) {
    box-sizing: border-box;
  }

  .state,
  .notification-list {
    width: 100%;
    height: 100%;
    min-height: 0;
  }

  .state {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 20px;
    text-align: center;
  }

  .state h2 {
    margin: 0 0 8px;
    color: #ef4444;
    font-size: 18px;
    font-weight: 600;
    text-align: center;
  }

  .loading-text,
  .error-detail,
  .empty-text {
    margin: 0;
    color: #6b7280;
  }

  .loading-text {
    margin-top: 12px;
    font-size: 16px;
  }

  .error-detail {
    font-size: 14px;
  }

  .empty-text {
    font-size: 16px;
  }

  .spinner {
    width: 2rem;
    height: 2rem;
    margin-bottom: 0.75rem;
    border: 0.25rem solid #e5e7eb;
    border-top-color: #6366f1;
    border-radius: 50%;
    animation: spin 0.8s linear infinite;
  }

  .notification-list {
    flex: 1;
    overflow-y: auto;
    margin: 0;
    padding: 16px;
    padding-bottom: 32px;
    list-style: none;
  }

  .notification-item {
    margin-bottom: 12px;
    padding: 16px;
    border-left: 4px solid #6366f1;
    border-radius: 12px;
    background: #f9fafb;
  }

  .notification-item h2,
  .notification-item p,
  .notification-item time {
    margin: 0;
  }

  .notification-item h2 {
    margin-bottom: 6px;
    color: #111827;
    font-size: 16px;
    font-weight: 600;
  }

  .notification-item p {
    margin-bottom: 8px;
    color: #4b5563;
    font-size: 14px;
    line-height: 20px;
  }

  .notification-item time {
    color: #9ca3af;
    font-size: 12px;
  }

  @keyframes spin {
    to {
      transform: rotate(360deg);
    }
  }
</style>
