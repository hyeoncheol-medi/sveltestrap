<script>
  import { run } from 'svelte/legacy';

  import { onDestroy, onMount } from 'svelte';
  import { createPopper } from '@popperjs/core';
  import { classnames, uuid } from '../utils';
  import { InlineContainer } from '../InlineContainer';
  import { Portal } from '../Portal';

  /**
   * Additional CSS class names for the tooltip.
   * @type {string}
   */
  

  

  

  
  

  

  

  

  

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {boolean} [animation] - Flag to enable animation for the tooltip.
   * @property {string} [children] - The content to be displayed within the tooltip.
   * @property {string} [container] - The container in which the tooltip should be rendered.
   * @property {string} [id] - Unique identifier for the tooltip.
   * @property {boolean} [isOpen] - Controls the visibility of the tooltip.
   * @property {string} [placement] - The preferred placement of the tooltip.
   * @property {string | HTMLElement} [target] - The target element to which the tooltip is attached.
   * @property {string | null} [theme] - The theme name override to apply to this component instance.
   * @property {string | number} [delay] - The delay for showing the tooltip (in milliseconds).
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    animation = true,
    children = '',
    container = undefined,
    id = `tooltip_${uuid()}`,
    isOpen = $bindable(false),
    placement = 'top',
    target = '',
    theme = null,
    delay = 0,
    children,
    ...rest
  } = $props();

  /**
   * @type {string}
   */
  let bsPlacement = $state();
  /**
   * @type {object}
   */
  let popperInstance = $state();
  /**
   * @type {string}
   */
  let popperPlacement = $state(placement);
  /**
   * @type {HTMLDivElement | null}
   */
  let targetEl = $state();
  /**
   * @type {HTMLDivElement | null}
   */
  let tooltipEl = $state();
  /**
   * @type {string}
   */
  let showTimer;

  const checkPopperPlacement = {
    name: 'checkPopperPlacement',
    enabled: true,
    phase: 'main',
    // @ts-ignore
    fn({ state }) {
      popperPlacement = state.placement;
    }
  };


  const open = () => {
    clearTimeout(showTimer);
    showTimer = setTimeout(() => (isOpen = true), delay);
  };

  const close = () => {
    clearTimeout(showTimer);
    isOpen = false;
  };

  onMount(registerEventListeners);

  onDestroy(() => {
    unregisterEventListeners();
    clearTimeout(showTimer);
  });


  function registerEventListeners() {
    // eslint-disable-next-line eqeqeq
    if (target == null || !target) {
      targetEl = null;
      return;
    }

    // Check if target is HTMLElement
    try {
      if (target instanceof HTMLElement) {
        // @ts-ignore
        targetEl = target;
      }
    } catch (e) {
      // fails on SSR
    }

    // If targetEl has not been found yet
    // eslint-disable-next-line eqeqeq
    if (targetEl == null) {
      // Check if target can be found via querySelector
      try {
        targetEl = document.querySelector(`#${target}`);
      } catch (e) {
        // fails on SSR
      }
    }

    // If we've found targetEl
    if (targetEl) {
      targetEl.addEventListener('mouseover', open);
      targetEl.addEventListener('mouseleave', close);
      targetEl.addEventListener('focus', open);
      targetEl.addEventListener('blur', close);
    }
  }

  function unregisterEventListeners() {
    if (targetEl) {
      targetEl.removeEventListener('mouseover', open);
      targetEl.removeEventListener('mouseleave', close);
      targetEl.removeEventListener('focus', open);
      targetEl.removeEventListener('blur', close);
      targetEl.removeAttribute('aria-describedby');
    }
  }




  run(() => {
    if (isOpen && tooltipEl) {
      // @ts-ignore
      popperInstance = createPopper(targetEl, tooltipEl, {
        placement,
        modifiers: [checkPopperPlacement]
      });
    } else if (popperInstance) {
      // @ts-ignore
      popperInstance.destroy();
      // @ts-ignore
      popperInstance = undefined;
    }
  });
  run(() => {
    if (target) {
      unregisterEventListeners();
      registerEventListeners();
    }
  });
  run(() => {
    if (targetEl) {
      if (isOpen) {
        targetEl.setAttribute('aria-describedby', id);
      } else {
        targetEl.removeAttribute('aria-describedby');
      }
    }
  });
  run(() => {
    if (popperPlacement === 'left') {
      bsPlacement = 'start';
    } else if (popperPlacement === 'right') {
      bsPlacement = 'end';
    } else {
      bsPlacement = popperPlacement;
    }
  });
  let classes = $derived(classnames(
    className,
    'tooltip',
    `bs-tooltip-${bsPlacement}`,
    animation ? 'fade' : false,
    isOpen ? 'show' : false
  ));
  let outer = $derived(container === 'inline' ? InlineContainer : Portal);
</script>

{#if isOpen}
  {@const SvelteComponent = outer}
  <SvelteComponent>
    <div
      bind:this={tooltipEl}
      {...rest}
      class={classes}
      {id}
      role="tooltip"
      data-bs-theme={theme}
      data-bs-delay={delay}
      x-placement={popperPlacement}
    >
      <div class="tooltip-arrow" data-popper-arrow></div>
      <div class="tooltip-inner">
        {#if children}
          {children}
        {:else}
          {@render children?.()}
        {/if}
      </div>
    </div>
  </SvelteComponent>
{/if}
