<script>
  import { classnames } from '../utils';

  // Additional CSS class name for the component

  /**
   * Additional CSS class name for the component
   * @type {string}
   */
  

  

  

  

  

  

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {boolean} [sm] - Small size setting for the component.
   * @property {boolean} [md] - Medium size setting for the component.
   * @property {boolean} [lg] - Large size setting for the component.
   * @property {boolean} [xl] - Extra-large size setting for the component.
   * @property {boolean} [xxl] - Double-extra-large size setting for the component.
   * @property {boolean} [fluid] - Flag indicating if the component should have a fluid width.
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    sm = undefined,
    md = undefined,
    lg = undefined,
    xl = undefined,
    xxl = undefined,
    fluid = false,
    children,
    ...rest
  } = $props();

  let classes = $derived(classnames(className, {
    'container-sm': sm,
    'container-md': md,
    'container-lg': lg,
    'container-xl': xl,
    'container-xxl': xxl,
    'container-fluid': fluid,
    container: !sm && !md && !lg && !xl && !xxl && !fluid
  }));
</script>

<div {...rest} class={classes}>
  {@render children?.()}
</div>
