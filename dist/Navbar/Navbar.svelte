<script>
  import { run } from 'svelte/legacy';

  import { setContext } from 'svelte';
  import { classnames } from '../utils';
  import { Container } from '../Container';

  setContext('navbar', {
    inNavbar: true
  });

  /**
   * Additional CSS class names for the navbar.
   * @type {string}
   */
  

  

  

  

  

  

  

  

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {'fluid' | 'sm' | 'md' | 'lg' | 'xl' | 'xxl'} [container] - Determines the container type within the navbar.
   * @property {string} [color] - The color theme for the navbar.
   * @property {boolean} [dark] - Flag to indicate if dark theme should be applied.
   * @property {boolean | string} [expand] - Determines when the navbar should expand.
   * @property {string} [fixed] - Fixes the navbar at the top or bottom of the viewport.
   * @property {boolean} [light] - Flag to indicate if light theme should be applied.
   * @property {string} [sticky] - Makes the navbar stick to the top of the viewport.
   * @property {string | null} [theme] - The theme name override to apply to this component instance.
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    container = 'fluid',
    color = '',
    dark = false,
    expand = false || '',
    fixed = '',
    light = false,
    sticky = '',
    theme = $bindable(null),
    children,
    ...rest
  } = $props();

  let containerProps = {
    sm: container === 'sm',
    md: container === 'md',
    lg: container === 'lg',
    xl: container === 'xl',
    xxl: container === 'xxl',
    fluid: container === 'fluid'
  };

  /**
   * Returns the class for the current expanded state
   * @param {boolean | string} expand - The expand prop value.
   * @returns {string | false} The class name or false if not applicable.
   */
  function getExpandClass(expand) {
    if (expand === false) {
      return false;
    }

    if (expand === true || expand === 'xs') {
      return 'navbar-expand';
    }

    return `navbar-expand-${expand}`;
  }

  run(() => {
    theme = dark ? 'dark' : light ? 'light' : theme;
  });

  let classes = $derived(classnames(className, 'navbar', getExpandClass(expand), {
    [`bg-${color}`]: color,
    [`fixed-${fixed}`]: fixed,
    [`sticky-${sticky}`]: sticky
  }));
</script>

<nav {...rest} class={classes} data-bs-theme={theme}>
  {#if container}
    <Container {...containerProps}>
      {@render children?.()}
    </Container>
  {:else}
    {@render children?.()}
  {/if}
</nav>
