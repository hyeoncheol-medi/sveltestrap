<script>
  import { createBubbler, handlers } from 'svelte/legacy';

  const bubble = createBubbler();
  import { getContext } from 'svelte';
  import { classnames } from '../utils';

  const context = getContext('dropdownContext');

  

  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {boolean} [active]
   * @property {boolean} [disabled]
   * @property {boolean} [divider]
   * @property {boolean} [header]
   * @property {boolean} [toggle]
   * @property {string} [href]
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    active = false,
    disabled = false,
    divider = false,
    header = false,
    toggle = true,
    href = '',
    children,
    ...rest
  } = $props();

  let classes = $derived(classnames(className, {
    disabled,
    'dropdown-item': !divider && !header,
    active: active,
    'dropdown-header': header,
    'dropdown-divider': divider
  }));

  function handleItemClick(e) {
    if (disabled || header || divider) {
      e.preventDefault();
      return;
    }

    if (toggle && ($context.autoClose === true || $context.autoClose === 'inside')) {
      $context.toggle(e);
    }
  }
</script>

<li>
  {#if header}
    <h6 {...rest} class={classes}>
      {@render children?.()}
    </h6>
  {:else if divider}
    <!-- svelte-ignore a11y_no_static_element_interactions -->
    <div {...rest} onclick={handlers(bubble('click'), handleItemClick)} class={classes}>
      {@render children?.()}
    </div>
  {:else if href}
    <a {...rest} click onclick={handleItemClick} {href} class={classes}>
      {@render children?.()}
    </a>
  {:else}
    <button type="button" {...rest} onclick={handlers(bubble('click'), handleItemClick)} class={classes}>
      {@render children?.()}
    </button>
  {/if}
</li>
