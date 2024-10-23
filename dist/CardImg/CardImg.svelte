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
   * @property {boolean} [top] - Place image at the top of the card.
   * @property {boolean} [bottom] - Place image at the bottom of the card.
   * @property {string} src - The image URL.
   * @property {string} [alt] - The image alt text.
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    top = false,
    bottom = false,
    src,
    alt = '',
    ...rest
  } = $props();

  let classes = $state('');

  run(() => {
    let cardImgClassName = 'card-img';

    if (top) {
      cardImgClassName = 'card-img-top';
    }

    if (bottom) {
      cardImgClassName = 'card-img-bottom';
    }

    classes = classnames(className, cardImgClassName);
  });
</script>

<img {...rest} class={classes} {src} {alt} />
