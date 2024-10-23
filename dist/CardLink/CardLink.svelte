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
   * @property {string} [href] - The URL to visit when clicked.
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let { class: className = '', href = '', children, ...rest } = $props();

  let classes = $derived(classnames(className, 'card-link'));
</script>

<a {...rest} class={classes} {href}>
  {@render children?.()}
</a>
