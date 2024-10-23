<script>
  import { createBubbler } from 'svelte/legacy';

  const bubble = createBubbler();
  import { classnames } from '../utils';

  /**
   * Additional CSS classes for the <button> element.
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
  

  let classes = $derived(classnames(className, 'accordion-button'));
</script>

<h2 class="accordion-header" {...rest}>
  <button type="button" class={classes} onclick={bubble('click')}>
    {@render children?.()}
  </button>
</h2>
