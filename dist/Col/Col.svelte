<script>
  import { getColumnSizeClass, isObject } from '../utils';

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {any} [xs]
   * @property {any} [sm]
   * @property {any} [md]
   * @property {any} [lg]
   * @property {any} [xl]
   * @property {any} [xxl]
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    xs = undefined,
    sm = undefined,
    md = undefined,
    lg = undefined,
    xl = undefined,
    xxl = undefined,
    children,
    ...rest
  } = $props();

  const colClasses = [];
  const lookup = {
    xs,
    sm,
    md,
    lg,
    xl,
    xxl
  };

  Object.keys(lookup).forEach((colWidth) => {
    const columnProp = lookup[colWidth];
    if (!columnProp && columnProp !== '') {
      return; //no value for this width
    }

    const isXs = colWidth === 'xs';

    if (isObject(columnProp)) {
      const colSizeInterfix = isXs ? '-' : `-${colWidth}-`;
      const colClass = getColumnSizeClass(isXs, colWidth, columnProp.size);

      if (columnProp.size || columnProp.size === '') {
        colClasses.push(colClass);
      }
      if (columnProp.push) {
        colClasses.push(`push${colSizeInterfix}${columnProp.push}`);
      }
      if (columnProp.pull) {
        colClasses.push(`pull${colSizeInterfix}${columnProp.pull}`);
      }
      if (columnProp.offset) {
        colClasses.push(`offset${colSizeInterfix}${columnProp.offset}`);
      }
      if (columnProp.order) {
        colClasses.push(`order${colSizeInterfix}${columnProp.order}`);
      }
    } else {
      colClasses.push(getColumnSizeClass(isXs, colWidth, columnProp));
    }
  });

  if (!colClasses.length) {
    colClasses.push('col');
  }

  if (className) {
    colClasses.push(className);
  }
</script>

<div {...rest} class={colClasses.join(' ')}>
  {@render children?.()}
</div>
