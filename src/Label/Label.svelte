<script>
  import { classnames, getColumnSizeClass, isObject } from '../utils';

  // Use $props rune for all props
  let {
    class: className = '',
    hidden = false,
    check = false,
    size = '',
    for: fore = null,
    xs = '',
    sm = '',
    md = '',
    lg = '',
    xl = '',
    xxl = '',
    ...rest
  } = $props();

  // Define column widths object
  const colWidths = {
    xs,
    sm,
    md,
    lg,
    xl,
    xxl
  };

  let widths = Object.keys(colWidths);
  const colClasses = [];

  $effect(() => {
    colClasses.length = 0; // Clear the array

    widths.forEach((colWidth) => {
      // Access the column prop from our defined props
      let columnProp = colWidths[colWidth];

      if (!columnProp && columnProp !== '') {
        return;
      }

      const isXs = colWidth === 'xs';
      let colClass;

      if (isObject(columnProp)) {
        const colSizeInterfix = isXs ? '-' : `-${colWidth}-`;
        colClass = getColumnSizeClass(isXs, colWidth, columnProp.size);

        colClasses.push(
          classnames({
            [colClass]: columnProp.size || columnProp.size === '',
            [`order${colSizeInterfix}${columnProp.order}`]: columnProp.order || columnProp.order === 0,
            [`offset${colSizeInterfix}${columnProp.offset}`]: columnProp.offset || columnProp.offset === 0
          })
        );
      } else {
        colClass = getColumnSizeClass(isXs, colWidth, columnProp);
        colClasses.push(colClass);
      }
    });
  });

  // Reactive classes calculation

  let classes = $derived(() => classnames(
    className,
    hidden ? 'visually-hidden' : false,
    check ? 'form-check-label' : false,
    size ? `col-form-label-${size}` : false,
    colClasses,
    colClasses.length ? 'col-form-label' : 'form-label'
  ))
</script>

<label {...rest} class={classes} for={fore}>
  <slot />
</label>
