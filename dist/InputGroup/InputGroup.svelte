<script>
  import { classnames } from '../utils';

  /**
   * Additional CSS class name for the component
   * @type {string}
   * @default ''
   */
  

  

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {string} [size] - Size of the component.
   * @property {string | null} [theme] - The theme name override to apply to this component instance.
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    size = '',
    theme = null,
    children,
    ...rest
  } = $props();

  let classes = $derived(classnames(className, 'input-group', size ? `input-group-${size}` : null));
</script>

<div {...rest} class={classes} data-bs-theme={theme}>
  {@render children?.()}
</div>
