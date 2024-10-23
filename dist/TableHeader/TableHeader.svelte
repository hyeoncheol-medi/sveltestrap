<script>
  import { setContext } from 'svelte';
  /**
   * @typedef {Object} Props
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let { children, ...rest } = $props();

  setContext('header', true);
</script>

<thead {...rest}>
  <tr>
    {@render children?.()}
  </tr>
</thead>
