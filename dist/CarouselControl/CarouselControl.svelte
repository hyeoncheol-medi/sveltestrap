<script>
  import { run, preventDefault } from 'svelte/legacy';

  import { classnames, getNewCarouselActiveIndex } from '../utils';

  /**
   * Additional CSS classes for container element.
   * @type {string}
   * @default ''
   */
  

  

  

  

  

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {string} [direction] - The direction of carousel control, can be 'next' or 'prev'.
   * @property {string} [directionText] - The text for screen readers, indicating the direction.
   * @property {number} [activeIndex] - The active index of the carousel.
   * @property {array} [items] - The items in the carousel.
   * @property {boolean} [wrap] - Determines whether the carousel should cycle continuously or not.
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    direction = '',
    directionText = '',
    activeIndex = $bindable(0),
    items = [],
    wrap = true,
    ...rest
  } = $props();

  let classes = $state('');
  let screenText = $state('');

  run(() => {
    classes = classnames(`carousel-control-${direction}`, className);
  });
  run(() => {
    screenText = directionText ? directionText : direction === 'next' ? 'Next' : 'Previous';
  });

  function clickHandler() {
    const endOrBeginning =
      (direction === 'next' && activeIndex + 1 > items.length - 1) || (direction === 'prev' && activeIndex - 1 < 0);

    if (!wrap && endOrBeginning) {
      return;
    }

    activeIndex = getNewCarouselActiveIndex(direction, items, activeIndex);
  }
</script>

<a {...rest} class={classes} role="button" href="#{direction}" onclick={preventDefault(clickHandler)}>
  <span class="carousel-control-{direction}-icon" aria-hidden="true"></span>
  <span class="visually-hidden">{screenText}</span>
</a>
