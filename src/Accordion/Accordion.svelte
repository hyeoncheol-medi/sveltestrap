<script>
  import { createEventDispatcher, setContext } from 'svelte';
  import { writable } from 'svelte/store';

  import { classnames } from '../utils';

  const dispatch = createEventDispatcher();

  /**
   * Additional CSS classes for container element.
   * @type {string}
   * @default ''
   */
  

  

  

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {boolean} [flush] - Determines if the accordion should have no visible border or background.
   * @property {boolean} [stayOpen] - Controls whether the accordion should stay open when an item is clicked.
   * @property {string | undefined} [theme] - The theme name override to apply to this component instance.
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    flush = false,
    stayOpen = false,
    theme = undefined,
    children,
    ...rest
  } = $props();

  const open = writable(null);

  setContext('accordion', {
    open,
    stayOpen,
    /**
     * Toggles the open state of the accordion based on the provided ID.
     * @param {HTMLDivElement} element - The accordion item element
     */
    toggle: (element) => {
      if ($open === element) {
        open.set(null);
      } else {
        open.set(element);
      }

      dispatch('toggle', {
        [element]: $open === element
      });
    }
  });

  let classes = $derived(classnames(className, 'accordion', { 'accordion-flush': flush }));
</script>

<div class={classes} data-bs-theme={theme} {...rest}>
  {@render children?.()}
</div>
