<script>
  import { run } from 'svelte/legacy';

  /**
   * @typedef {Object} Props
   * @property {boolean} [icons]
   * @property {any} [theme]
   */

  /** @type {Props} */
  let { icons = true, theme = undefined } = $props();

  run(() => {
    if (typeof document !== 'undefined' && theme !== undefined) {
      if (theme === 'auto' && window.matchMedia('(prefers-color-scheme: dark)').matches) {
        document.documentElement.setAttribute('data-bs-theme', 'dark');
      } else {
        document.documentElement.setAttribute('data-bs-theme', theme);
      }
    }
  });
</script>

<svelte:head>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" />

  {#if icons}
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.2/font/bootstrap-icons.min.css" />
  {/if}
</svelte:head>
