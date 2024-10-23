<script>
  import { createBubbler } from 'svelte/legacy';

  const bubble = createBubbler();
  import { classnames } from '../utils';

  /**
   * Additional CSS classes for container element.
   * @type {string}
   * @default ''
   */
  

  

  

  

  

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {boolean} [body] - Determines whether to include a card body.
   * @property {string} [color] - Specifies the color theme of the card.
   * @property {boolean} [inverse] - Inverts the text color of the card.
   * @property {boolean} [outline] - Render the card with an outline style.
   * @property {string | undefined} [theme] - The theme name override to apply to this component instance.
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    body = false,
    color = '',
    inverse = false,
    outline = false,
    theme = undefined,
    children,
    ...rest
  } = $props();

  let classes = $derived(classnames(
    className,
    'card',
    inverse ? 'text-white' : false,
    body ? 'card-body' : false,
    color ? `${outline ? 'border' : 'bg'}-${color}` : false
  ));
</script>

<!-- svelte-ignore a11y_no_static_element_interactions -->
<div {...rest} data-bs-theme={theme} class={classes} onclick={bubble('click')}>
  {@render children?.()}
</div>
