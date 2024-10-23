<script>
  import { classnames } from '../utils';

  /**
   * Additional CSS class name for the component
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
  

  let classes = $derived(classnames(className, 'input-group-text'));
</script>

<span {...rest} class={classes}>
  {@render children?.()}
</span>
