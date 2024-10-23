<script>
  import { classnames } from '../utils';

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {any} [children]
   * @property {string} [closeAriaLabel]
   * @property {any} [toggle]
   * @property {import('svelte').Snippet} [children]
   * @property {import('svelte').Snippet} [close]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    children = undefined,
    closeAriaLabel = 'Close',
    toggle = undefined,
    children,
    close,
    ...rest
  } = $props();

  let classes = $derived(classnames(className, 'offcanvas-header'));
</script>

<div {...rest} class={classes}>
  <h5 class="offcanvas-title">
    {#if children}
      {children}
    {:else}
      {@render children?.()}
    {/if}
  </h5>
  {#if close}{@render close()}{:else}
    {#if typeof toggle === 'function'}
      <button aria-label={closeAriaLabel} class="btn-close" onclick={toggle} type="button"></button>
    {/if}
  {/if}
</div>
