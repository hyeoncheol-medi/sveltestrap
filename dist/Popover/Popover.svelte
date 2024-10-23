<script>
  import { onMount } from 'svelte';
  import { createPopper } from '@popperjs/core';
  import { classnames } from '../utils';
  import { InlineContainer } from '../InlineContainer';
  import { Portal } from '../Portal';

  let {
    class: className = '',
    animation = true,
    children = '',
    container = undefined,
    dismissible = false,
    hideOnOutsideClick = false,
    isOpen = false,
    placement = 'top',
    target = '',
    theme = null,
    title = '',
    trigger = 'click',
    titleContent,
    defaultContent,
    ...restProps
  } = $props();

  let targetEl;
  let popoverEl;
  let popperInstance = $state(undefined);
  let bsPlacement;
  let popperPlacement = $state(placement);

  const checkPopperPlacement = {
    name: 'checkPopperPlacement',
    enabled: true,
    phase: 'main',
    fn({ state }) {
      popperPlacement = state.placement;
    }
  };

  $effect(() => {
    if (isOpen && popoverEl) {
      popperInstance = createPopper(targetEl, popoverEl, {
        placement,
        modifiers: [
          checkPopperPlacement,
          {
            name: 'offset',
            options: {
              offset: () => {
                return [0, 8];
              }
            }
          }
        ]
      });
    } else if (popperInstance) {
      popperInstance.destroy();
      popperInstance = undefined;
    }
  });

  const open = () => (isOpen = true);
  const close = () => (isOpen = false);
  const toggle = () => (isOpen = !isOpen);

  onMount(() => {
    targetEl = document.querySelector(`#${target}`);

    switch (trigger) {
      case 'hover':
        targetEl.addEventListener('mouseover', open);
        targetEl.addEventListener('mouseleave', close);
        break;
      case 'focus':
        targetEl.addEventListener('focus', open);
        targetEl.addEventListener('blur', close);
        break;
      default:
        targetEl.addEventListener('click', toggle);

        if (dismissible) {
          targetEl.addEventListener('blur', close);
        }

        break;
    }

    return () => {
      switch (trigger) {
        case 'hover':
          targetEl.removeEventListener('mouseover', open);
          targetEl.removeEventListener('mouseleave', close);
          break;
        case 'focus':
          targetEl.removeEventListener('focus', open);
          targetEl.removeEventListener('blur', close);
          break;
        default:
          targetEl.removeEventListener('click', toggle);

          if (dismissible) {
            targetEl.removeEventListener('blur', close);
          }
          break;
      }
    };
  });

  const handleOutsideClick = (event) => {
    if (isOpen && hideOnOutsideClick && !popoverEl.contains(event.target)) {
      isOpen = false;
    }
  };

  $effect(() => {
    if (!target) {
      throw new Error('Need target!');
    }
  });

  $effect(() => {
    if (popperPlacement === 'left') {
      bsPlacement = 'start';
    } else if (popperPlacement === 'right') {
      bsPlacement = 'end';
    } else {
      bsPlacement = popperPlacement;
    }
  });

  const classes = $derived(
    classnames(
      className,
      'popover',
      animation ? 'fade' : false,
      `bs-popover-${bsPlacement}`,
      isOpen ? 'show' : false
    )
  );

  const outer = $derived(container === 'inline' ? InlineContainer : Portal);
</script>

<svelte:window onmousedown={handleOutsideClick} />

{#if isOpen}
  <svelte:component this={outer}>
    <div
      bind:this={popoverEl}
      {...restProps}
      class={classes}
      role="tooltip"
      data-bs-theme={theme}
      x-placement={popperPlacement}
    >
      <div class="popover-arrow" data-popper-arrow />
      <h3 class="popover-header">
        {@render titleContent?.() ?? title}
      </h3>
      <div class="popover-body">
        {#if children}
          {children}
        {:else}
          {@render defaultContent?.()}
        {/if}
      </div>
    </div>
  </svelte:component>
{/if}
