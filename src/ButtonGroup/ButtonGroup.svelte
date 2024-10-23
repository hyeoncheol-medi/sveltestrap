<script>
  import { classnames } from '../utils';

  /**
   * Additional CSS classes for container element.
   * @type {string}
   * @default ''
   */
  

  

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {string} [size] - Size of the Button Group.
   * @property {boolean} [vertical] - Determines if the Button Group should appear vertically.
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    size = '',
    vertical = false,
    children,
    ...rest
  } = $props();

  let classes = $derived(classnames(className, size ? `btn-group-${size}` : false, vertical ? 'btn-group-vertical' : 'btn-group'));
</script>

<div {...rest} class={classes}>
  {@render children?.()}
</div>
