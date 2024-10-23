<script>
  import { Dropdown } from '../Dropdown';
  /**
   * @typedef {Object} Props
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let { children, ...rest } = $props();
</script>

<Dropdown {...rest} group on:click>
  {@render children?.()}
</Dropdown>
