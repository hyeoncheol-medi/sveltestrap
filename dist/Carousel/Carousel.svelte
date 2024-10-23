<script>
  import { run } from 'svelte/legacy';

  import { onDestroy, onMount } from 'svelte';

  import { browserEvent, classnames, getNewCarouselActiveIndex } from '../utils';

  /**
   * Additional CSS classes for container element.
   * @type {string}
   * @default ''
   */
  

  

  

  

  

  

  

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {number} [activeIndex] - The index of the currently active item in the carousel.
   * @property {number} [interval] - The time interval (in milliseconds) between automatic transitions of the carousel items.
   * @property {Array} [items] - An array of items to be displayed in the carousel.
   * @property {boolean} [keyboard] - A boolean indicating whether the carousel should respond to keyboard navigation.
   * @property {boolean} [pause] - A boolean indicating whether automatic cycling of the carousel should pause on hover.
   * @property {boolean} [ride] - A boolean indicating whether the carousel should automatically cycle through items.
   * @property {string | undefined} [theme] - The theme name override to apply to this component instance.
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    activeIndex = $bindable(0),
    interval = 5000,
    items = [],
    keyboard = true,
    pause = true,
    ride = true,
    theme = undefined,
    children,
    ...rest
  } = $props();

  let _rideTimeoutId = false;
  let _removeVisibilityChangeListener = false;
  let classes = $state('');

  run(() => {
    classes = classnames(className, 'carousel', 'slide');
  });

  onMount(() => {
    setRideTimeout();

    _removeVisibilityChangeListener = browserEvent(document, 'visibilitychange', () => {
      if (document.visibilityState === 'hidden') {
        clearRideTimeout();
      } else {
        setRideTimeout();
      }
    });
  });

  onDestroy(() => {
    if (_rideTimeoutId) {
      clearTimeout(_rideTimeoutId);
    }

    if (_removeVisibilityChangeListener) {
      _removeVisibilityChangeListener();
    }
  });

  function handleKeydown(event) {
    if (!keyboard) {
      return;
    }

    let direction = '';

    if (event.key === 'ArrowLeft') {
      direction = 'prev';
    } else if (event.key === 'ArrowRight') {
      direction = 'next';
    } else {
      return;
    }

    activeIndex = getNewCarouselActiveIndex(direction, items, activeIndex);
  }

  function setRideTimeout() {
    clearRideTimeout();

    if (ride) {
      _rideTimeoutId = setTimeout(autoNext, interval);
    }
  }

  function clearRideTimeout() {
    if (_rideTimeoutId) {
      clearTimeout(_rideTimeoutId);
    }
  }

  function autoNext() {
    activeIndex = getNewCarouselActiveIndex('next', items, activeIndex);
  }
</script>

<svelte:window onkeydown={handleKeydown} />

<div
  {...rest}
  role="presentation"
  class={classes}
  data-bs-theme={theme}
  onmouseenter={() => (pause ? clearRideTimeout() : undefined)}
  onmouseleave={() => (pause ? setRideTimeout() : undefined)}
>
  {@render children?.()}
</div>
