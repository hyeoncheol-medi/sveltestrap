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
   * @property {string} [name] - Name of the icon to display.
   */

  /** @type {Props & { [key: string]: any }} */
  let { class: className = '', name = '', ...rest } = $props();

  let classes = $derived(classnames(className, `bi-${name}`));
</script>

<i {...rest} class={classes}></i>
