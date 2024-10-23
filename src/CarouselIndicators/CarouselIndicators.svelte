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
   * @property {any[]} [items] - Carousel items.
   * @property {number} [activeIndex] - Active carousel item index.
   */

  /** @type {Props & { [key: string]: any }} */
  let { class: className = '', items = [], activeIndex = $bindable(0), ...rest } = $props();

  let classes = $state('');

  run(() => {
    classes = classnames(className, 'carousel-indicators');
  });
</script>

<div {...rest} class={classes}>
  {#each items as item, index}
    <button
      data-bs-target
      class:active={activeIndex === index}
      aria-current={activeIndex === index}
      aria-label={item.title}
      onclick={() => (activeIndex = index)}
    >
      {item.title ? item.title : ''}
    </button>
  {/each}
</div>
