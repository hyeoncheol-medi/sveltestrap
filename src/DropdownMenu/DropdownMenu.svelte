<script>
  import { getContext } from 'svelte';
  import { classnames } from '../utils';

  const context = getContext('dropdownContext');

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {boolean} [end]
   * @property {boolean} [right]
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    end = false,
    right = false,
    children,
    ...rest
  } = $props();

  const popperPlacement = (direction, end) => {
    let prefix = direction;

    if (direction === 'up') {
      prefix = 'top';
    }

    if (direction === 'down') {
      prefix = 'bottom';
    }

    let suffix = end ? 'end' : 'start';
    return `${prefix}-${suffix}`;
  };

  let popperOptions = $derived({
    modifiers: [
      { name: 'flip' },
      {
        name: 'offset',
        options: {
          offset: [0, 2]
        }
      }
    ],
    placement: popperPlacement($context.direction, end || right)
  });

  let classes = $derived(classnames(className, 'dropdown-menu', {
    'dropdown-menu-end': end || right,
    show: $context.isOpen
  }));
</script>

<ul
  {...rest}
  class={classes}
  data-bs-popper={$context.inNavbar ? 'static' : undefined}
  use:$context.popperContent={popperOptions}
>
  {@render children?.()}
</ul>
