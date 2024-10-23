<script>
  import { classnames } from '../utils';

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {boolean} [inline]
   * @property {any} [color]
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    inline = false,
    color = undefined,
    children,
    ...rest
  } = $props();

  let classes = $derived(classnames(className, !inline ? 'form-text' : false, color ? `text-${color}` : false));
</script>

<small {...rest} class={classes}>
  {@render children?.()}
</small>
