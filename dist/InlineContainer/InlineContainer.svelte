<script>
  /**
   * @typedef {Object} Props
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props} */
  let { children } = $props();
  let x = 'wtf svelte?'; // eslint-disable-line
</script>

<div>{@render children?.()}</div>
