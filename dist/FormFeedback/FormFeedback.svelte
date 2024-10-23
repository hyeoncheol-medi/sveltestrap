<script>
  import { run } from 'svelte/legacy';

  import { classnames } from '../utils';

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {any} [valid]
   * @property {boolean} [tooltip]
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    valid = undefined,
    tooltip = false,
    children,
    ...rest
  } = $props();
  let classes = $state();

  run(() => {
    const validMode = tooltip ? 'tooltip' : 'feedback';

    classes = classnames(className, valid ? `valid-${validMode}` : `invalid-${validMode}`);
  });
</script>

<div {...rest} class={classes}>
  {@render children?.()}
</div>
