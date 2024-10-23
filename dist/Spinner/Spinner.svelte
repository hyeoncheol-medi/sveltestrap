<script>
  import { classnames } from '../utils';

  // Additional CSS class name for the component

  /**
   * Additional CSS class name for the component
   * @type {string}
   */
  

  

  

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {string} [type] - Type of spinner border.
   * @property {string} [size] - Size of the spinner.
   * @property {string} [color] - Color of the spinner.
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    type = 'border',
    size = '',
    color = '',
    children,
    ...rest
  } = $props();

  let classes = $derived(classnames(
    className,
    size ? `spinner-${type}-${size}` : false,
    `spinner-${type}`,
    color ? `text-${color}` : false
  ));
</script>

<div {...rest} role="status" class={classes}>
  <span class="visually-hidden">
    {#if children}{@render children()}{:else}Loading...{/if}
  </span>
</div>
