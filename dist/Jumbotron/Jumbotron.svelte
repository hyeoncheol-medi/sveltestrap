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

  /** @type {Props} */
  let { class: className = '', children } = $props();
  

  let classes = $derived(classnames(className, 'p-5 mb-4 bg-light rounded-3'));
</script>

<div class={classes}>
  {@render children?.()}
</div>
