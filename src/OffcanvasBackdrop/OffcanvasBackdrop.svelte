<script>
  import { createBubbler } from 'svelte/legacy';

  const bubble = createBubbler();
  // TODO only allow single backdrop open at once.
  import { classnames } from '../utils';
  import { backdropIn, backdropOut } from '../transitions.js';
  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {boolean} [isOpen]
   * @property {boolean} [fade]
   */

  /** @type {Props & { [key: string]: any }} */
  let { class: className = '', isOpen = false, fade = true, ...rest } = $props();
  let classes = $derived(classnames(className, 'offcanvas-backdrop'));
</script>

{#if isOpen}
  <div role="presentation" {...rest} onclick={bubble('click')} class={classes} class:fade in:backdropIn out:backdropOut></div>
{/if}
