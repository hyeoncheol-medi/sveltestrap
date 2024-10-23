<script>
  import { createBubbler, handlers } from 'svelte/legacy';

  const bubble = createBubbler();
  import { getContext } from 'svelte';
  import { classnames } from '../utils';

  const context = getContext('dropdownContext');

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {string} [ariaLabel]
   * @property {boolean} [active]
   * @property {boolean} [block]
   * @property {boolean} [caret]
   * @property {string} [color]
   * @property {boolean} [disabled]
   * @property {any} [inner]
   * @property {boolean} [nav]
   * @property {boolean} [outline]
   * @property {string} [size]
   * @property {boolean} [split]
   * @property {any} [tag]
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    ariaLabel = 'Toggle Dropdown',
    active = false,
    block = false,
    caret = false,
    color = 'secondary',
    disabled = false,
    inner = $bindable(undefined),
    nav = false,
    outline = false,
    size = '',
    split = false,
    tag = null,
    children,
    ...rest
  } = $props();

  let classes = $derived(classnames(className, {
    'dropdown-toggle': caret || split,
    'dropdown-toggle-split': split,
    'nav-link': nav,
    show: $context.isOpen
  }));

  function toggleButton(e) {
    if (disabled) {
      e.preventDefault();
      return;
    }

    if (nav) {
      e.preventDefault();
    }

    $context.toggle(e);
  }

  let btnClasses = $derived(classnames(
    classes,
    'btn',
    `btn${outline ? '-outline' : ''}-${color}`,
    size ? `btn-${size}` : false,
    block ? 'd-block w-100' : false,
    { active }
  ));
</script>

{#if nav}
  <a
    use:$context.popperRef
    {...rest}
    bind:this={inner}
    onclick={handlers(bubble('click'), toggleButton)}
    href="#nav"
    aria-expanded={$context.isOpen}
    class={classes}
  >
    {#if children}{@render children()}{:else}
      <span class="visually-hidden">{ariaLabel}</span>
    {/if}
  </a>
{:else if tag === 'div'}
  <!-- svelte-ignore a11y_no_static_element_interactions -->
  <div
    use:$context.popperRef
    {...rest}
    bind:this={inner}
    onclick={handlers(bubble('click'), toggleButton)}
    aria-expanded={$context.isOpen}
    class={classes}
  >
    {#if children}{@render children()}{:else}
      <span class="visually-hidden">{ariaLabel}</span>
    {/if}
  </div>
{:else if tag === 'span'}
  <!-- svelte-ignore a11y_no_static_element_interactions -->
  <span
    use:$context.popperRef
    {...rest}
    bind:this={inner}
    onclick={handlers(bubble('click'), toggleButton)}
    aria-expanded={$context.isOpen}
    class={classes}
  >
    {#if children}{@render children()}{:else}
      <span class="visually-hidden">{ariaLabel}</span>
    {/if}
  </span>
{:else}
  <button
    use:$context.popperRef
    {...rest}
    bind:this={inner}
    type="button"
    onclick={handlers(bubble('click'), toggleButton)}
    aria-expanded={$context.isOpen}
    class={btnClasses}
  >
    {#if children}{@render children()}{:else}
      <span class="visually-hidden">{ariaLabel}</span>
    {/if}
  </button>
{/if}
