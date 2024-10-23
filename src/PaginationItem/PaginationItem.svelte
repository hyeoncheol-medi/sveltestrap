<script>
  import { classnames } from '../utils';

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {boolean} [active]
   * @property {boolean} [disabled]
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    active = false,
    disabled = false,
    children,
    ...rest
  } = $props();

  let classes = $derived(classnames(className, 'page-item', {
    active,
    disabled
  }));
</script>

<li {...rest} class={classes}>
  {@render children?.()}
</li>
