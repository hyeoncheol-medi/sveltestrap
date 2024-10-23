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
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let { class: className = '', children, ...rest } = $props();
  

  let classes = $derived(classnames(className, 'card-img-overlay'));
</script>

<div {...rest} class={classes}>
  {@render children?.()}
</div>
