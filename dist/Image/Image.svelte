<script>
  import { getContext } from 'svelte';
  import { classnames } from '../utils';

  /**
   * Additional CSS class name for the component
   * @type {string}
   * @default ''
   */
  

  

  

  

  

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {string | undefined} [alt] - Alternative text for the image
   * @property {boolean} [figure] - Determines if the image should apply the figure class
   * @property {boolean} [fluid] - Fluid image indicator flag
   * @property {string | null} [theme] - The theme name override to apply to this component instance.
   * @property {boolean} [thumbnail] - Thumbnail image indicator flag
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    alt = undefined,
    figure = getContext('figure'),
    fluid = false,
    theme = null,
    thumbnail = false,
    ...rest
  } = $props();

  let classes = $derived(classnames(className, {
    'figure-img': figure,
    'img-fluid': fluid,
    'img-thumbnail': thumbnail
  }));
</script>

<img {alt} {...rest} data-bs-theme={theme} class={classes} />
