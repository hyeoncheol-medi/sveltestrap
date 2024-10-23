<script>
  import { classnames } from '../utils';

  

  /**
   * Additional CSS class name for the component
   * @type {string}
   */
  

  

  

  
  /**
   * @typedef {Object} Props
   * @property {string} [ariaLabel] - The ARIA label for the pagination.
   * @property {string} [class]
   * @property {string} [listClassName] - The class name for the pagination list.
   * @property {string} [size] - The size of the pagination.
   * @property {string | null} [theme] - The theme name override to apply to this component instance.
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    ariaLabel = 'pagination',
    class: className = '',
    listClassName = '',
    size = '',
    theme = null,
    children,
    ...rest
  } = $props();

  let classes = $derived(classnames(className));

  let listClasses = $derived(classnames(listClassName, 'pagination', {
    [`pagination-${size}`]: !!size
  }));
</script>

<nav {...rest} class={classes} aria-label={ariaLabel} data-bs-theme={theme}>
  <ul class={listClasses}>
    {@render children?.()}
  </ul>
</nav>
