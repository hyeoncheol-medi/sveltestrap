<script>
  import { createBubbler, handlers } from 'svelte/legacy';

  const bubble = createBubbler();
  import { createEventDispatcher, onMount } from 'svelte';
  import { fade } from 'svelte/transition';
  import toggle from '../toggle';

  const dispatch = createEventDispatcher();

  

  // Additional CSS class names for styling.
  /**
   * Additional CSS class names for styling.
   * @type {string}
   */
  

  

  

  

  

  
  /**
   * @typedef {Object} Props
   * @property {boolean} [isOpen] - Tracks the open/closed state of the Fade component.
   * @property {string} [class]
   * @property {Function} [onEntering] - Callback triggered before Fade enters; dispatches 'opening'.
   * @property {Function} [onEntered] - Callback triggered after Fade has entered; dispatches 'open'.
   * @property {Function} [onExiting] - Callback triggered before Fade exits; dispatches 'closing'.
   * @property {Function} [onExited] - Callback triggered after Fade has exited; dispatches 'close'.
   * @property {HTMLElement | null} [toggler] - Manages the state of a toggling element associated with the Fade component.
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    isOpen = $bindable(false),
    class: className = '',
    onEntering = () => dispatch('opening'),
    onEntered = () => dispatch('open'),
    onExiting = () => dispatch('closing'),
    onExited = () => dispatch('close'),
    toggler = null,
    children,
    ...rest
  } = $props();

  onMount(() =>
    toggle(toggler, (e) => {
      isOpen = !isOpen;
      e.preventDefault();
    })
  );
</script>

{#if isOpen}
  <div
    {...rest}
    transition:fade|local
    onintrostart={handlers(bubble('introstart'), onEntering)}
    onintroend={handlers(bubble('introend'), onEntered)}
    onoutrostart={handlers(bubble('outrostart'), onExiting)}
    onoutroend={handlers(bubble('outroend'), onExited)}
    class={className}
  >
    {@render children?.()}
  </div>
{/if}
