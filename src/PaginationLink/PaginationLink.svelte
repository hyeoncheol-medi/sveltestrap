<script>
  import { run, createBubbler } from 'svelte/legacy';

  const bubble = createBubbler();
  import { classnames } from '../utils';

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {boolean} [next]
   * @property {boolean} [previous]
   * @property {boolean} [first]
   * @property {boolean} [last]
   * @property {string} [ariaLabel]
   * @property {string} [href]
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    next = false,
    previous = false,
    first = false,
    last = false,
    ariaLabel = '',
    href = '',
    children,
    ...rest
  } = $props();

  let classes = $derived(classnames(className, 'page-link'));

  let defaultAriaLabel = $state();

  run(() => {
    if (previous) {
      defaultAriaLabel = 'Previous';
    } else if (next) {
      defaultAriaLabel = 'Next';
    } else if (first) {
      defaultAriaLabel = 'First';
    } else if (last) {
      defaultAriaLabel = 'Last';
    }
  });

  let realLabel = $derived(ariaLabel || defaultAriaLabel);

  let defaultCaret = $state();
  run(() => {
    if (previous) {
      defaultCaret = '\u2039';
    } else if (next) {
      defaultCaret = '\u203A';
    } else if (first) {
      defaultCaret = '\u00ab';
    } else if (last) {
      defaultCaret = '\u00bb';
    }
  });
</script>

<a {...rest} class={classes} onclick={bubble('click')} {href}>
  {#if previous || next || first || last}
    <span aria-hidden="true">
      {#if children}{@render children()}{:else}{defaultCaret}{/if}
    </span>
    <span class="visually-hidden">{realLabel}</span>
  {:else}
    {@render children?.()}
  {/if}
</a>
