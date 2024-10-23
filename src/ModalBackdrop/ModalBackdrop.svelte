<script>
  import { createBubbler } from 'svelte/legacy';

  const bubble = createBubbler();
  import { onMount } from 'svelte';
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

  let loaded = $state(false);

  onMount(() => {
    loaded = true;
  });

  let classes = $derived(classnames(className, 'modal-backdrop'));
</script>

{#if isOpen && loaded}
  <div role="presentation" {...rest} class={classes} class:fade in:backdropIn out:backdropOut onclick={bubble('click')}></div>
{/if}
