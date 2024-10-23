<script>
  import { onMount, onDestroy } from 'svelte';
  /**
   * @typedef {Object} Props
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let { children, ...rest } = $props();
  let ref = $state();
  let portal;

  onMount(() => {
    portal = document.createElement('div');
    document.body.appendChild(portal);
    portal.appendChild(ref);
  });

  onDestroy(() => {
    if (portal) {
      document.body.removeChild(portal);
    }
  });
</script>

<div bind:this={ref} {...rest}>
  {@render children?.()}
</div>
