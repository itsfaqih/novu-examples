<script lang="ts">
  import { onMount } from 'svelte';
  import type { Novu, NovuOptions } from '@novu/js';
  import type { NovuUI } from '@novu/js/ui';

  export let applicationIdentifier: string;
  export let subscriber: string;

  let inboxElement: HTMLElement | undefined;

  const options = {
    applicationIdentifier,
    subscriber
  } satisfies NovuOptions;

  onMount(() => {
    let disposed = false;
    let novu: Novu | undefined;
    let novuUI: NovuUI | undefined;

    void Promise.all([import('@novu/js'), import('@novu/js/ui')]).then(([{ Novu }, { NovuUI }]) => {
      const element = inboxElement;
      if (disposed || !element) return;

      novu = new Novu(options);
      novuUI = new NovuUI({
        options,
        novu,
        appearance: {
          variables: {
            colorPrimary: '#6366f1',
            colorForeground: '#111827'
          }
        }
      });

      novuUI.mountComponent({
        name: 'Inbox',
        element
      });
    });

    return () => {
      disposed = true;
      const element = inboxElement;

      if (novuUI && element) {
        novuUI.unmountComponent(element);
      }

      novuUI?.unmount();
      void novu?.socket.disconnect();
    };
  });
</script>

<div class="inbox-host" bind:this={inboxElement}></div>

<style>
  .inbox-host {
    display: grid;
    min-width: 2.75rem;
    min-height: 2.75rem;
    place-items: center;
  }
</style>

