<script>
  import { createBubbler } from 'svelte/legacy';

  const bubble = createBubbler();
  import { classnames } from '../utils';

  /**
   * Additional CSS classes for container element.
   * @type {string}
   * @default ''
   */
  

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {string} [tag] - HTML tag to use for container element.
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let { class: className = '', tag = 'div', children, ...rest } = $props();

  let classes = $derived(classnames(className, 'card-header'));
</script>

{#if tag === 'h3'}
  <!-- svelte-ignore a11y_no_noninteractive_element_interactions -->
  <h3 {...rest} class={classes} onclick={bubble('click')}>
    {@render children?.()}
  </h3>
{:else}
  <!-- svelte-ignore a11y_no_static_element_interactions -->
  <div {...rest} class={classes} onclick={bubble('click')}>
    {@render children?.()}
  </div>
{/if}
