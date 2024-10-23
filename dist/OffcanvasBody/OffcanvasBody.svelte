<script>
  import { classnames } from '../utils';

  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let { class: className = '', children, ...rest } = $props();
  

  let classes = $derived(classnames(className, 'offcanvas-body'));
</script>

<div {...rest} class={classes}>
  {@render children?.()}
</div>
