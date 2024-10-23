<script>
  import { run, createBubbler, handlers } from 'svelte/legacy';

  const bubble = createBubbler();
  import { createEventDispatcher, onMount } from 'svelte';
  import { collapseIn, collapseOut } from '../transitions';
  import { classnames } from '../utils';
  import toggle from '../toggle';

  const dispatch = createEventDispatcher();

  
  /**
   * @typedef {Object} Props
   * @property {boolean} [isOpen]
   * @property {string} [class]
   * @property {boolean} [horizontal]
   * @property {boolean} [navbar]
   * @property {any} [onEntering]
   * @property {any} [onEntered]
   * @property {any} [onExiting]
   * @property {any} [onExited]
   * @property {boolean} [expand]
   * @property {any} [toggler]
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    isOpen = $bindable(false),
    class: className = '',
    horizontal = false,
    navbar = false,
    onEntering = () => dispatch('opening'),
    onEntered = () => dispatch('open'),
    onExiting = () => dispatch('closing'),
    onExited = () => dispatch('close'),
    expand = false,
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

  let classes = $derived(classnames(className, {
    'collapse-horizontal': horizontal,
    'navbar-collapse': navbar
  }));

  let windowWidth = $state(0);
  let _wasMaximized = $state(false);

  // TODO wrong to hardcode these here - come from Bootstrap CSS only
  const minWidth = $state({});
  minWidth['xs'] = 0;
  minWidth['sm'] = 576;
  minWidth['md'] = 768;
  minWidth['lg'] = 992;
  minWidth['xl'] = 1200;

  function notify() {
    dispatch('update', isOpen);
  }

  run(() => {
    if (navbar && expand) {
      if (windowWidth >= minWidth[expand] && !isOpen) {
        isOpen = true;
        _wasMaximized = true;
        notify();
      } else if (windowWidth < minWidth[expand] && _wasMaximized) {
        isOpen = false;
        _wasMaximized = false;
        notify();
      }
    }
  });
</script>

<svelte:window bind:innerWidth={windowWidth} />

{#if isOpen}
  <div
    style={navbar ? undefined : 'overflow: hidden;'}
    {...rest}
    class={classes}
    in:collapseIn={{ horizontal }}
    out:collapseOut|local={{ horizontal }}
    onintrostart={handlers(bubble('introstart'), onEntering)}
    onintroend={handlers(bubble('introend'), onEntered)}
    onoutrostart={handlers(bubble('outrostart'), onExiting)}
    onoutroend={handlers(bubble('outroend'), onExited)}
  >
    {@render children?.()}
  </div>
{/if}
