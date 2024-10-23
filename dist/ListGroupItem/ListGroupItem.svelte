<script>
  import { createBubbler } from 'svelte/legacy';

  const bubble = createBubbler();
  import { classnames } from '../utils';

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {boolean} [active]
   * @property {boolean} [disabled]
   * @property {string} [color]
   * @property {boolean} [action]
   * @property {any} [href]
   * @property {any} [tag]
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    active = false,
    disabled = false,
    color = '',
    action = false,
    href = null,
    tag = null,
    children,
    ...rest
  } = $props();

  let classes = $derived(classnames(className, 'list-group-item', {
    active,
    disabled,
    'list-group-item-action': action || tag === 'button',
    [`list-group-item-${color}`]: color
  }));
</script>

{#if href}
  <a {...rest} class={classes} onclick={bubble('click')} {href} {disabled} {active}>
    {@render children?.()}
  </a>
{:else if tag === 'button'}
  <button {...rest} class={classes} type="button" onclick={bubble('click')} {disabled} {active}>
    {@render children?.()}
  </button>
{:else}
  <!-- svelte-ignore a11y_click_events_have_key_events -->
  <!-- svelte-ignore a11y_no_noninteractive_element_interactions -->
  <li {...rest} class={classes} onclick={bubble('click')} {disabled} {active}>
    {@render children?.()}
  </li>
{/if}
