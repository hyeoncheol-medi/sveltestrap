<script>
  import { setContext } from 'svelte';
  import { Nav } from '../Nav';
  /**
   * @typedef {Object} Props
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let { children, ...rest } = $props();

  setContext('tabs', true);
</script>

<Nav {...rest}>
  {@render children?.()}
</Nav>
