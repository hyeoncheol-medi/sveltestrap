<script>
  import { classnames } from '../utils';

  /**
   * Additional CSS class names for the nav item.
   * @type {string}
   */
  

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {boolean} [active] - Indicates whether the nav item is active.
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let { class: className = '', active = false, children, ...rest } = $props();

  let classes = $derived(classnames(className, 'nav-item', active ? 'active' : false));
</script>

<li {...rest} class={classes}>
  {@render children?.()}
</li>
