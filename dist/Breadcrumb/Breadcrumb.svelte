<script>
  // Importing a utility function for handling class names.
  import { classnames } from '../utils';

  /**
   * Additional CSS class names for the <nav> element.
   * @type {string}
   * @default ''
   */
  

  

  

  

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {string} [children] - The content to be displayed within the breadcrumb.
   * @property {string} [divider] - Custom divider character or string for the breadcrumb items.
   * @property {string} [listClassName] - Additional CSS class names for the <ol> element inside the breadcrumb.
   * @property {string} [style] - Inline styles for the breadcrumb.
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    children = '',
    divider = '',
    listClassName = '',
    style = '',
    children,
    ...rest
  } = $props();

  let listClasses = $derived(classnames('breadcrumb', listClassName));
  let styles = $derived(divider ? `--bs-breadcrumb-divider: '${divider}'; ${style || ''}` : style);
</script>

<nav style={styles} {...rest} class={className}>
  <ol class={listClasses}>
    {#if children}
      {children}
    {:else}
      {@render children?.()}
    {/if}
  </ol>
</nav>
