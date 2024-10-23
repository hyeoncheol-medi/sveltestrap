<script>
  import { classnames } from '../utils';

  /**
   * Additional CSS class names for the <li> element.
   * @type {string}
   * @default ''
   */





  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {boolean} [active] - Indicates whether the breadcrumb item is active (i.e., the current page).
   * @property {string} [children] - The content to be displayed within the breadcrumb item.
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    active = false,
    children = '',
    ...rest
  } = $props();

  let classes = $derived(classnames(className, active ? 'active' : false, 'breadcrumb-item'));
</script>

<li {...rest} class={classes} aria-current={active ? 'page' : undefined}>
  {#if children}
    {children}
  {:else}
    {@render children?.()}
  {/if}
</li>
