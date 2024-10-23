<script>
  import { createEventDispatcher, onMount } from 'svelte';

  import { InlineContainer } from '../InlineContainer';
  import { OffcanvasBackdrop } from '../OffcanvasBackdrop';
  import { OffcanvasBody } from '../OffcanvasBody';
  import { OffcanvasHeader } from '../OffcanvasHeader';
  import { Portal } from '../Portal';

  import { classnames, browserEvent, getTransitionDuration } from '../utils';

  const dispatch = createEventDispatcher();

  let {
    class: className = '',
    backdrop = true,
    body = true,
    container = 'body',
    fade = true,
    header = '',
    isOpen = false,
    keyboard = true,
    placement = 'start',
    scroll = false,
    sm = false,
    md = false,
    lg = false,
    xl = false,
    xxl = false,
    style = '',
    theme = null,
    toggle = undefined,
    ...restProps
  } = $props();

  let {
    header: headerContent,
    default: defaultContent
  } = $props();

  let bodyElement = $state(null);
  let isTransitioning = $state(false);
  let element = $state(null);
  let removeEscListener = $state(null);

  onMount(() => {
    bodyElement = document.body;
  });

  $effect(() => {
    if (bodyElement && !scroll) {
      bodyElement.classList.toggle('overflow-noscroll', isOpen || isTransitioning);
    }
  });

  $effect(() => {
    if (element) {
      isOpen = isOpen;
      isTransitioning = true;

      dispatch(isOpen ? 'opening' : 'closing');

      setTimeout(() => {
        isTransitioning = false;
        dispatch(isOpen ? 'open' : 'close');
      }, getTransitionDuration(element));
    }
  });

  $effect(() => {
    if (isOpen && toggle && typeof window !== 'undefined') {
      removeEscListener = browserEvent(document, 'keydown', (event) => {
        if (event.key && event.key === 'Escape' && keyboard) {
          toggle();
        }
      });
    }
  });

  $effect(() => {
    if (!isOpen && removeEscListener) {
      removeEscListener();
    }
  });

  $derived handleMouseDown = backdrop && toggle && bodyElement && isOpen
    ? (e) => {
        if (e.target === bodyElement) {
          toggle();
        }
      }
    : undefined;

  $derived classes = classnames(
    {
      offcanvas: !sm && !md && !lg && !xl && !xxl,
      'offcanvas-sm': sm,
      'offcanvas-md': md,
      'offcanvas-lg': lg,
      'offcanvas-xl': xl,
      'offcanvas-xxl': xxl,
      show: isOpen
    },
    `offcanvas-${placement}`,
    className
  );

  $derived outer = container === 'inline' ? InlineContainer : Portal;
</script>

<svelte:body onmousedown={handleMouseDown} />

<svelte:component this={outer}>
  <div
    {...restProps}
    bind:this={element}
    aria-hidden={!isOpen ? true : undefined}
    aria-modal={isOpen ? true : undefined}
    class={classes}
    role={isOpen ? 'dialog' : undefined}
    data-bs-theme={theme}
    style={`visibility: ${isOpen || isTransitioning ? 'visible' : 'hidden'};${style}`}
    tabindex="-1"
  >
    {#if toggle || header || headerContent}
      <OffcanvasHeader {toggle}>
        {#if header}
          {header}
        {/if}
        {@render headerContent?.()}
      </OffcanvasHeader>
    {/if}

    {#if body}
      <OffcanvasBody>
        {@render defaultContent?.()}
      </OffcanvasBody>
    {:else}
      {@render defaultContent?.()}
    {/if}
  </div>

  {#if backdrop}
    <OffcanvasBackdrop onclick={toggle || undefined} {fade} {isOpen} />
  {/if}
</svelte:component>

<style>
  :global(.overflow-noscroll) {
    overflow: hidden;
    padding-right: 0px;
  }
</style>
