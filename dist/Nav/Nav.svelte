<script>
  import { classnames } from '../utils';

  /**
   * Additional CSS class names for the <ul> element.
   * @type {string}
   */
  

  

  

  

  

  

  

  

  

  

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {boolean} [tabs] - Indicates whether the nav should be styled as tabs.
   * @property {boolean} [pills] - Indicates whether the nav should be styled as pills.
   * @property {string} [vertical] - Controls vertical orientation of the nav.
   * @property {string} [horizontal] - Controls horizontal alignment of the nav items.
   * @property {boolean} [justified] - Indicates whether the nav should be justified.
   * @property {boolean} [fill] - Indicates whether the nav should fill the available space.
   * @property {boolean} [navbar] - Indicates whether the nav is part of a navbar.
   * @property {boolean} [card] - Indicates whether the nav is part of a card header.
   * @property {string | null} [theme] - The theme name override to apply to this component instance.
   * @property {boolean} [underline] - Indicates whether the nav should have an underline style.
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    tabs = false,
    pills = false,
    vertical = '',
    horizontal = '',
    justified = false,
    fill = false,
    navbar = false,
    card = false,
    theme = null,
    underline = false,
    children,
    ...rest
  } = $props();

  /**
   * Computes the class for vertical orientation based on the provided value.
   * @param {boolean | 'xs' | 'sm' | 'md' | 'lg' | 'xl'} vertical - The vertical prop value.
   * @returns {string | false} The class name or false if not applicable.
   */
  function getVerticalClass(vertical) {
    if (!vertical) {
      return false;
    }

    if (vertical === true || vertical === 'xs') {
      return 'flex-column';
    }

    return `flex-${vertical}-column`;
  }

  /**
   * Computed class names for the <ul> element based on the component's props.
   * @type {string}
   */
  let classes = $derived(classnames(
    className,
    navbar ? 'navbar-nav' : 'nav',
    horizontal ? `justify-content-${horizontal}` : false,
    getVerticalClass(vertical),
    {
      'nav-tabs': tabs,
      'card-header-tabs': card && tabs,
      'nav-pills': pills,
      'card-header-pills': card && pills,
      'nav-justified': justified,
      'nav-fill': fill,
      'nav-underline': underline
    }
  ));
</script>

<ul {...rest} class={classes} data-bs-theme={theme}>
  {@render children?.()}
</ul>
