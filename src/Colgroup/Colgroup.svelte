<script>
  import { setContext } from 'svelte';
  /**
   * @typedef {Object} Props
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props} */
  let { children } = $props();

  setContext('colgroup', true);
</script>

<colgroup>
  {@render children?.()}
</colgroup>
