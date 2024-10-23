<script>
  import { classnames } from '../utils';

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {boolean} [responsive]
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props} */
  let { class: className = '', responsive = false, children } = $props();

  let responsiveClassName = $derived(classnames(className, {
    'table-responsive': responsive === true,
    [`table-responsive-${responsive}`]: typeof responsive === 'string'
  }));
</script>

{#if responsive}
  <div class={responsiveClassName}>{@render children?.()}</div>
{:else}
  {@render children?.()}
{/if}
