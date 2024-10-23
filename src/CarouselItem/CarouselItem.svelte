<script>
  import { run } from 'svelte/legacy';

  import { classnames } from '../utils';

  /**
   * Additional CSS classes for container element.
   * @type {string}
   * @default ''
   */
  

  

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {number} [activeIndex] - Index of the active item.
   * @property {number} [itemIndex] - Index of the current item.
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    activeIndex = 0,
    itemIndex = 0,
    children,
    ...rest
  } = $props();

  let classes = $state('');

  run(() => {
    classes = classnames(className, 'carousel-item');
  });
</script>

<div {...rest} class="{classes} active" class:active={itemIndex === activeIndex}>
  {@render children?.()}
</div>
