<script>
  import { classnames } from '../utils';

  /**
   * Additional CSS class names to apply
   * @type {string}
   */








  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {boolean | undefined} [toggle] - Determines whether the modal header includes a close button.
   * @property {string} [closeAriaLabel] - The aria-label for the close button.
   * @property {string} [id] - The unique id of the modal header.
   * @property {any} [children]
   * @property {import('svelte').Snippet} [children]
   * @property {import('svelte').Snippet} [close]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    toggle = undefined,
    closeAriaLabel = 'Close',
    id = undefined,
    children = undefined,
    close,
    ...rest
  } = $props();

  let classes = $derived(classnames(className, 'modal-header'));
</script>

<div {...rest} class={classes}>
  <h5 class="modal-title" {id}>
    {#if children}
      {children}
    {:else}
      {@render children?.()}
    {/if}
  </h5>
  {#if close}{@render close()}{:else}
    {#if typeof toggle === 'function'}
      <button type="button" onclick={toggle} class="btn-close" aria-label={closeAriaLabel}></button>
    {/if}
  {/if}
</div>
