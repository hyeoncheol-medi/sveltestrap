<script>
  import { createBubbler } from 'svelte/legacy';

  const bubble = createBubbler();
  import { classnames } from '../utils';

  /**
   * Additional CSS class names for the navbar brand.
   * @type {string}
   */
  

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {string} [href] - The URL to link to when the navbar brand is clicked.
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let { class: className = '', href = '/', children, ...rest } = $props();

  let classes = $derived(classnames(className, 'navbar-brand'));
</script>

<a {...rest} class={classes} {href} onclick={bubble('click')}>
  {@render children?.()}
</a>
