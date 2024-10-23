<script>
  import { run } from 'svelte/legacy';

  import { useColorMode } from './helpers';

  /**
   * @typedef {Object} Props
   * @property {string} [theme]
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props} */
  let { theme = 'dark', children } = $props();

  let ref = $state(null);

  run(() => {
    ref && useColorMode(ref, theme);
  });
</script>

<span bind:this={ref}>
  {@render children?.()}
</span>

<style>
  span {
    display: contents;
  }
</style>
