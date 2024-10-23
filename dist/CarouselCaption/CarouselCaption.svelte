<script>
  import { run } from 'svelte/legacy';

  import { classnames } from '../utils';

  /**
   * Additional CSS classes for container element.
   * @type {string}
   * @default ''
   */
  

  

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {string} [captionHeader] - Header text for caption.
   * @property {string} [captionText] - Text for caption.
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    captionHeader = '',
    captionText = '',
    children,
    ...rest
  } = $props();

  let classes = $state('');

  run(() => {
    classes = classnames(className, 'carousel-caption', 'd-none', 'd-md-block');
  });
</script>

<div {...rest} class={classes}>
  {#if captionHeader}<h5>{captionHeader}</h5>{/if}
  {#if captionText}<p>{captionText}</p>{/if}
  {@render children?.()}
</div>
