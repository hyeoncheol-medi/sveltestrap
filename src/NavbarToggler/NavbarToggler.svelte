<script>
  import { createBubbler } from 'svelte/legacy';

  const bubble = createBubbler();
  import { classnames } from '../utils';

  /**
   * Additional CSS class names for the toggler.
   * @type {string}
   */
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let { class: className = '', children, ...rest } = $props();
  

  let classes = $derived(classnames(className, 'navbar-toggler'));
</script>

<button {...rest} onclick={bubble('click')} class={classes}>
  {#if children}{@render children()}{:else}
    <span class="navbar-toggler-icon"></span>
  {/if}
</button>
