<script>
  import { classnames } from '../utils';

  /**
   * Additional CSS class names to apply
   * @type {string}
   */
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let { class: className = '', children, ...rest } = $props();
  

  let classes = $derived(classnames(className, 'modal-footer'));
</script>

<div {...rest} class={classes}>
  {@render children?.()}
</div>
