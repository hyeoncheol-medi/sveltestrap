<script>
  import { colorMode, toggleColorMode } from './helpers';
  /**
   * @typedef {Object} Props
   * @property {import('svelte').Snippet<[any]>} [children]
   */

  /** @type {Props} */
  let { children } = $props();

  let currentColorMode = $state($colorMode);

  colorMode.subscribe((value) => {
    currentColorMode = value;
  });
</script>

{@render children?.({ currentColorMode, toggleColorMode, })}
