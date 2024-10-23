<script>
  import { run } from 'svelte/legacy';

  import { getContext, setContext, onDestroy } from 'svelte';
  import { createPopperActions } from '../popper';
  import { classnames } from '../utils';

  import { createContext } from './DropdownContext';

  const noop = () => undefined;

  let context = createContext();
  setContext('dropdownContext', context);
  const navbarContext = getContext('navbar');

  // Additional CSS class names to add to the container.

  /**
   * Exports a prop `class` which can be used to apply custom CSS classes.
   * @type {string}
   */
  

  

  

  

  

  

  

  

  

  

  

  

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {boolean} [active] - A boolean indicating whether the Dropdown is active (open).
   * @property {boolean} [autoClose] - A boolean indicating whether the Dropdown should automatically close when an item is selected.
   * @property {string} [direction] - The direction in which the Dropdown menu should expand ('down', 'up', 'left', 'right').
   * @property {boolean} [dropup] - A boolean indicating whether the Dropdown should expand upwards.
   * @property {boolean} [group] - A boolean indicating whether the Dropdown is part of a group.
   * @property {boolean} [inNavbar] - A boolean indicating whether the Dropdown is inside a Navbar.
   * @property {boolean} [isOpen] - A boolean indicating whether the Dropdown is currently open.
   * @property {boolean} [nav] - A boolean indicating whether the Dropdown is used in a navigation bar.
   * @property {boolean} [setActiveFromChild] - A boolean indicating whether the Dropdown should set its active state based on a child item's selection.
   * @property {string} [size] - The size variant for the Dropdown.
   * @property {string | null} [theme] - The theme name override to apply to this component instance.
   * @property {Function | undefined} [toggle] - A function that can be used to toggle the state of the Dropdown.
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    active = false,
    autoClose = true,
    direction = 'down',
    dropup = false,
    group = false,
    inNavbar = navbarContext ? navbarContext.inNavbar : false,
    isOpen = $bindable(false),
    nav = false,
    setActiveFromChild = false,
    size = '',
    theme = null,
    toggle = undefined,
    children,
    ...rest
  } = $props();

  const [popperRef, popperContent] = createPopperActions();

  const validDirections = ['up', 'down', 'left', 'right', 'start', 'end'];

  if (validDirections.indexOf(direction) === -1) {
    throw new Error(
      `Invalid direction sent: '${direction}' is not one of 'up', 'down', 'left', 'right', 'start', 'end'`
    );
  }

  let component = $state();
  let dropdownDirection = $state();







  function handleDocumentClick(e) {
    if (e && (e.which === 3 || (e.type === 'keyup' && e.which !== 9))) return;

    if (component.contains(e.target) && component !== e.target && (e.type !== 'keyup' || e.which === 9)) {
      return;
    }

    if (autoClose === true || autoClose === 'outside') {
      handleToggle(e);
    }
  }

  onDestroy(() => {
    if (typeof document !== 'undefined') {
      ['click', 'touchstart', 'keyup'].forEach((event) =>
        document.removeEventListener(event, handleDocumentClick, true)
      );
    }
  });
  let subItemIsActive = $derived(!!(
    setActiveFromChild &&
    component &&
    typeof component.querySelector === 'function' &&
    component.querySelector('.active')
  ));
  run(() => {
    if (direction === 'left') dropdownDirection = 'start';
    else if (direction === 'right') dropdownDirection = 'end';
    else dropdownDirection = direction;
  });
  let handleToggle = $derived(toggle || (() => (isOpen = !isOpen)));
  let classes = $derived(classnames(
    className,
    direction !== 'down' && `drop${dropdownDirection}`,
    nav && active ? 'active' : false,
    setActiveFromChild && subItemIsActive ? 'active' : false,
    {
      'btn-group': group,
      [`btn-group-${size}`]: !!size,
      dropdown: !group,
      show: isOpen,
      'nav-item': nav
    }
  ));
  run(() => {
    if (typeof document !== 'undefined') {
      if (isOpen) {
        ['click', 'touchstart', 'keyup'].forEach((event) =>
          document.addEventListener(event, handleDocumentClick, true)
        );
      } else {
        ['click', 'touchstart', 'keyup'].forEach((event) =>
          document.removeEventListener(event, handleDocumentClick, true)
        );
      }
    }
  });
  run(() => {
    context.update(() => {
      return {
        toggle: handleToggle,
        isOpen,
        autoClose,
        direction: direction === 'down' && dropup ? 'up' : direction,
        inNavbar: nav || inNavbar,
        popperRef: nav ? noop : popperRef,
        popperContent: nav ? noop : popperContent
      };
    });
  });
</script>

{#if nav}
  <li {...rest} class={classes} data-bs-theme={theme} bind:this={component}>
    {@render children?.()}
  </li>
{:else}
  <div {...rest} class={classes} data-bs-theme={theme} bind:this={component}>
    {@render children?.()}
  </div>
{/if}
