<script>
  import { run } from 'svelte/legacy';

  import { createEventDispatcher, onDestroy } from 'svelte';
  import { fade as fadeTrans } from 'svelte/transition';
  import { ToastBody } from '../ToastBody';
  import { ToastHeader } from '../ToastHeader';
  import { classnames } from '../utils';

  const dispatch = createEventDispatcher();

  /**
   * Additional CSS class names to add to the container.
   * @type {string}
   * @default ''
   */
  

  

  

  

  

  

  

  

  

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {boolean} [autohide] - Controls whether the Toast component autohides after a certain duration.
   * @property {boolean} [body] - Specifies whether the Toast component includes a body wrapper.
   * @property {number} [delay] - The time delay (in milliseconds) before the Toast component autohides.
   * @property {number} [duration] - The duration (in milliseconds) for the fade-in and fade-out animation of the Toast component.
   * @property {boolean} [fade] - Controls whether the Toast component fades in and out.
   * @property {string | undefined} [header] - Specifies the header content of the Toast component.
   * @property {boolean} [isOpen] - Controls whether the Toast component is initially open.
   * @property {string | null} [theme] - The theme name override to apply to this component instance.
   * @property {null} [toggle] - Function to toggle the visibility of the Toast component.
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    autohide = false,
    body = false,
    delay = 5000,
    duration = 200,
    fade = true,
    header = undefined,
    isOpen = $bindable(true),
    theme = null,
    toggle = null,
    children,
    ...rest
  } = $props();

  /**
   * The timer ID for the autohide timeout.
   * @type {number}
   */
  let timeout = $state();

  onDestroy(() => {
    return () => clearTimeout(timeout);
  });

  run(() => {
    if (isOpen && autohide) {
      // @ts-ignore
      timeout = setTimeout(() => (isOpen = false), delay);
    }
  });

  let classes = $derived(classnames(className, 'toast', {
    show: isOpen
  }));
</script>

{#if isOpen}
  <div
    {...rest}
    class={classes}
    data-bs-theme={theme}
    transition:fadeTrans={{ duration: fade && duration }}
    onintrostart={() => dispatch('opening')}
    onintroend={() => dispatch('open')}
    onoutrostart={() => dispatch('closing')}
    onoutroend={() => dispatch('close')}
    role="alert"
  >
    {#if header}
      <ToastHeader {toggle}>
        {header}
      </ToastHeader>
    {/if}
    {#if body}
      <ToastBody>
        {@render children?.()}
      </ToastBody>
    {:else}
      {@render children?.()}
    {/if}
  </div>
{/if}
