<script>
  import { classnames } from '../utils';

  

  

  

  /**
   * Additional CSS class name for the component
   * @type {string}
   */
  

  

  

  

  

  

  
  /**
   * @typedef {Object} Props
   * @property {boolean} [animated] - Indicates if the bar should have animation.
   * @property {boolean} [bar] - Indicates whether to show a bar.
   * @property {string} [barClassName] - The class name for the bar.
   * @property {string} [class]
   * @property {string} [color] - The color of the bar.
   * @property {number} [max] - The maximum value of the bar.
   * @property {boolean} [multi] - Indicates if it's a multi-bar.
   * @property {boolean} [striped] - Indicates if the bar should be striped.
   * @property {string | null} [theme] - The theme name override to apply to this component instance.
   * @property {number} [value] - The current value of the bar.
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    animated = false,
    bar = false,
    barClassName = '',
    class: className = '',
    color = '',
    max = 100,
    multi = false,
    striped = false,
    theme = null,
    value = 0,
    children,
    ...rest
  } = $props();

  let classes = $derived(classnames(className, 'progress'));

  let progressBarClasses = $derived(classnames(
    'progress-bar',
    bar ? className || barClassName : barClassName,
    animated ? 'progress-bar-animated' : null,
    color ? `text-bg-${color}` : null,
    striped || animated ? 'progress-bar-striped' : null
  ));

  let percent = $derived((parseInt(value, 10) / parseInt(max, 10)) * 100);
</script>

{#if bar}
  {#if multi}
    {@render children?.()}
  {:else}
    <div
      {...rest}
      class={progressBarClasses}
      style="width: {percent}%"
      data-bs-theme={theme}
      role="progressbar"
      aria-valuenow={value}
      aria-valuemin="0"
      aria-valuemax={max}
    >
      {@render children?.()}
    </div>
  {/if}
{:else}
  <div {...rest} data-bs-theme={theme} class={classes}>
    {#if multi}
      {@render children?.()}
    {:else}
      <div
        class={progressBarClasses}
        style="width: {percent}%"
        data-bs-theme={theme}
        role="progressbar"
        aria-valuenow={value}
        aria-valuemin="0"
        aria-valuemax={max}
      >
        {@render children?.()}
      </div>
    {/if}
  </div>
{/if}
