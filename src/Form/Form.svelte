<script>
  import { createBubbler } from 'svelte/legacy';

  const bubble = createBubbler();
  import { classnames } from '../utils';

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {boolean} [inline]
   * @property {boolean} [validated]
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    inline = false,
    validated = false,
    children,
    ...rest
  } = $props();

  let classes = $derived(classnames(className, {
    'form-inline': inline,
    'was-validated': validated
  }));
</script>

<form {...rest} class={classes} onsubmit={bubble('submit')}>
  {@render children?.()}
</form>
