<script>
  import { classnames } from '../utils';
  import { Colgroup } from '../Colgroup';
  import { ResponsiveContainer } from '../ResponsiveContainer';
  import { TableFooter } from '../TableFooter';
  import { TableHeader } from '../TableHeader';

  /**
   * Represents a custom class name for the component.
   * @type {string}
   */

  

  

  

  

  

  

  

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {string} [size] - Represents the size of the component.
   * @property {boolean} [bordered] - Indicates whether the component should have a bordered style.
   * @property {boolean} [borderless] - Indicates whether the component should have a borderless style.
   * @property {boolean} [striped] - Indicates whether the component should have a striped style.
   * @property {boolean} [hover] - Indicates whether the component should have a hover effect.
   * @property {boolean} [responsive] - Indicates whether the component should be responsive.
   * @property {Object[]} [rows] - Represents the number of rows for the component.
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    size = '',
    bordered = false,
    borderless = false,
    striped = false,
    hover = false,
    responsive = false,
    rows = undefined,
    children,
    ...rest
  } = $props();

  let classes = $derived(classnames(
    className,
    'table',
    size ? 'table-' + size : false,
    bordered ? 'table-bordered' : false,
    borderless ? 'table-borderless' : false,
    striped ? 'table-striped' : false,
    hover ? 'table-hover' : false
  ));
</script>

<ResponsiveContainer {responsive}>
  <table {...rest} class={classes}>
    {#if rows}
      <Colgroup>
        {@render children?.()}
      </Colgroup>
      <TableHeader>
        {@render children?.()}
      </TableHeader>
      <tbody>
        {#each rows as row}
          <tr>
            {@render children?.({ row, })}
          </tr>
        {/each}
      </tbody>
      <TableFooter>
        {@render children?.()}
      </TableFooter>
    {:else}
      {@render children?.()}
    {/if}
  </table>
</ResponsiveContainer>
