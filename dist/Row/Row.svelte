<script>
  import { classnames } from '../utils';

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {boolean} [noGutters]
   * @property {boolean} [form]
   * @property {number} [cols]
   * @property {any} [inner]
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    noGutters = false,
    form = false,
    cols = 0,
    inner = $bindable(undefined),
    children,
    ...rest
  } = $props();

  function getCols(cols) {
    const colsValue = parseInt(cols);
    if (!isNaN(colsValue)) {
      if (colsValue > 0) {
        return [`row-cols-${colsValue}`];
      }
    } else if (typeof cols === 'object') {
      return ['xs', 'sm', 'md', 'lg', 'xl']
        .map((colWidth) => {
          const isXs = colWidth === 'xs';
          const colSizeInterfix = isXs ? '-' : `-${colWidth}-`;
          const value = cols[colWidth];
          if (typeof value === 'number' && value > 0) {
            return `row-cols${colSizeInterfix}${value}`;
          }
          return null;
        })
        .filter((value) => !!value);
    }
    return [];
  }

  let classes = $derived(classnames(className, noGutters ? 'gx-0' : null, form ? 'form-row' : 'row', ...getCols(cols)));
</script>

<div {...rest} class={classes} bind:this={inner}>
  {@render children?.()}
</div>
