<script>
  import { tick } from 'svelte';
  import { modalIn, modalOut } from '../transitions';
  import { InlineContainer } from '../InlineContainer';
  import { ModalBackdrop } from '../ModalBackdrop';
  import { ModalBody } from '../ModalBody';
  import { ModalHeader } from '../ModalHeader';
  import { Portal } from '../Portal';

  import {
    browserEvent,
    classnames,
    conditionallyUpdateScrollbar,
    getOriginalBodyPadding,
    setScrollbarWidth,
    uuid
  } from '../utils';

  // Module context variables
  let openCount = $state(0);

  let {
    onOpen,
    onClose,
    onClosing,
    onOpening,
    autoFocus = true,
    body = false,
    centered = false,
    container = undefined,
    fullscreen = false,
    header = undefined,
    isOpen = $bindable(),
    keyboard = true,
    backdrop = true,
    contentClassName = '',
    fade = true,
    labelledBy = header ? `modal-${uuid()}` : undefined,
    modalClassName = '',
    modalStyle = null,
    returnFocusAfterClose = true,
    scrollable = false,
    size = '',
    theme = null,
    toggle = undefined,
    unmountOnClose = true,
    wrapClassName = ''
  } = $props();

  let className = '';
  export { className as class };

  let staticModal = false;
  export { staticModal as static };

  // State variables
  let hasOpened = $state(false);
  let _isMounted = $state(false);
  let _triggeringElement = $state(null);
  let _originalBodyPadding = $state(null);
  let _dialog = $state(null);
  let _mouseDownElement = $state(null);
  let _removeEscListener = $state(null);

  // Computed values
  const dialogBaseClass = 'modal-dialog';

  let classes = $derived(classnames(dialogBaseClass, className, {
    [`modal-${size}`]: size,
    'modal-fullscreen': fullscreen === true,
    [`modal-fullscreen-${fullscreen}-down`]: fullscreen && typeof fullscreen === 'string',
    [`${dialogBaseClass}-centered`]: centered,
    [`${dialogBaseClass}-scrollable`]: scrollable
  }))

  let outer = $derived(container === 'inline' || staticModal ? InlineContainer : Portal)


  // Mount 효과
  $effect(() => {
    if (isOpen) {
      init();
      hasOpened = true;
    }

    if (hasOpened && autoFocus) {
      setFocus();
    }

    return () => {
      destroy();
      if (hasOpened) {
        close();
      }
    };
  });

  // isOpen과 hasOpened 변경 감지 효과
  $effect(() => {
    if (isOpen) {
      init();
      hasOpened = true;
    }

    if (autoFocus && hasOpened) {
      setFocus();
    }
  });

  function setFocus() {
    if (_dialog && _dialog.parentNode && typeof _dialog.parentNode.focus === 'function') {
      _dialog.parentNode.focus();
    }
  }

  function init() {
    try {
      _triggeringElement = document.activeElement;
    } catch (err) {
      _triggeringElement = null;
    }

    if (!staticModal) {
      _originalBodyPadding = getOriginalBodyPadding();
      conditionallyUpdateScrollbar();
      if (openCount === 0) {
        document.body.className = classnames(document.body.className, 'modal-open');
      }

      ++openCount;
    }
    _isMounted = true;
  }

  function manageFocusAfterClose() {
    if (_triggeringElement) {
      if (typeof _triggeringElement.focus === 'function' && returnFocusAfterClose) {
        _triggeringElement.focus();
      }

      _triggeringElement = null;
    }
  }

  function destroy() {
    manageFocusAfterClose();
  }

  function close() {
    if (openCount <= 1) {
      document.body.classList.remove('modal-open');
    }

    manageFocusAfterClose();
    openCount = Math.max(0, openCount - 1);

    setScrollbarWidth(_originalBodyPadding);
  }

  function handleBackdropClick(e) {
    if (e.target === _mouseDownElement) {
      if (!isOpen || !backdrop) {
        return;
      }

      const backdropElem = _dialog ? _dialog.parentNode : null;
      if (backdrop === true && backdropElem && e.target === backdropElem && toggle) {
        e.stopPropagation();
        toggle(e);
      }
    }
  }

  function onModalOpened() {
    onOpen?.();
    _removeEscListener = browserEvent(document, 'keydown', (event) => {
      if (event.key && event.key === 'Escape' && keyboard) {
        if (toggle && backdrop === true) {
          if (_removeEscListener) _removeEscListener();
          toggle(event);
        }
      }
    });
  }

  function onModalClosing() {
    onClosing?.();
    if (_removeEscListener) {
      _removeEscListener();
    }
  }

  function onModalClosed() {
    onClose?.();
    if (unmountOnClose) {
      destroy();
    }
    close();
    if (_isMounted) {
      hasOpened = false;
    }
    _isMounted = false;
  }

  function handleBackdropMouseDown(e) {
    _mouseDownElement = e.target;
  }

</script>

{#if _isMounted}
  <svelte:component this={outer}>
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <div class={wrapClassName} tabindex="-1" {...rest} data-bs-theme={theme}>
      {#if isOpen}
        <!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
        <div
          in:modalIn|global
          out:modalOut|global
          style={modalStyle}
          aria-labelledby={labelledBy}
          class={classnames('modal', modalClassName, {
            fade,
            'position-static': staticModal
          })}
          role="dialog"
          on:introstart={() => dispatch('opening')}
          on:introend={onModalOpened}
          on:outrostart={onModalClosing}
          on:outroend={onModalClosed}
          on:click={handleBackdropClick}
          on:mousedown={handleBackdropMouseDown}
        >
          <slot name="external" />
          <div class={classes} role="document" bind:this={_dialog}>
            <div class={classnames('modal-content', contentClassName)}>
              {#if header}
                <ModalHeader {toggle} id={labelledBy}>
                  {header}
                </ModalHeader>
              {/if}
              {#if body}
                <ModalBody>
                  <slot />
                </ModalBody>
              {:else}
                <slot />
              {/if}
            </div>
          </div>
        </div>
      {/if}
    </div>
  </svelte:component>
{/if}

{#if backdrop && !staticModal}
  <svelte:component this={outer}>
    <ModalBackdrop {fade} {isOpen} />
  </svelte:component>
{/if}

<style>
  :global(.modal-open) {
    overflow: hidden;
    padding-right: 0;
  }
</style>
