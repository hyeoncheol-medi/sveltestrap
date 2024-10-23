<script>
  import { classnames } from '../utils';

  // Additional CSS class names to add to the container.

  /**
   * Exports a prop `class` which can be used to apply custom CSS classes.
   * @type {string}
   */
  

  

  

  

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {boolean} [flush] - Exports a prop `flush` which can be used to render list group items edge-to-edge in a parent container.
   * @property {boolean} [horizontal] - Exports a prop `horizontal` which can be used to render list group items side-by-side in a parent container (e.g., cards).
   * @property {boolean} [numbered] - Exports a prop `numbered` which can be used to render list group items with a number.
   * @property {string | null} [theme] - The theme name override to apply to this component instance.
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    flush = false,
    horizontal = false,
    numbered = false,
    theme = null,
    children,
    ...rest
  } = $props();

  let classes = $derived(classnames(className, 'list-group', {
    'list-group-flush': flush,
    'list-group-horizontal': horizontal,
    'list-group-numbered': numbered
  }));
</script>

{#if numbered}
  <ol {...rest} class={classes} data-bs-theme={theme}>
    {@render children?.()}
  </ol>
{:else}
  <ul {...rest} class={classes} data-bs-theme={theme}>
    {@render children?.()}
  </ul>
{/if}
