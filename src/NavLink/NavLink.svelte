<script>
  import { createBubbler, handlers } from 'svelte/legacy';

  const bubble = createBubbler();
  import { classnames } from '../utils';

  /**
   * Additional CSS class names for the nav link.
   * @type {string}
   */
  

  

  

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {boolean} [disabled] - Indicates whether the nav link is disabled.
   * @property {boolean} [active] - Indicates whether the nav link is active.
   * @property {string} [href] - The URL to link to when the nav link is clicked.
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    disabled = false,
    active = false,
    href = '#',
    children,
    ...rest
  } = $props();

  let classes = $derived(classnames(className, 'nav-link', {
    disabled,
    active
  }));

  function handleClick(e) {
    if (disabled) {
      e.preventDefault();
      e.stopImmediatePropagation();
      return;
    }

    if (href === '#') {
      e.preventDefault();
    }
  }
</script>

<a {...rest} {href} onclick={handlers(bubble('click'), handleClick)} class={classes}>
  {@render children?.()}
</a>
