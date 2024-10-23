<script>
  import Alert from './Alert.svelte';
  /**
   * @typedef {Object} Props
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let { children, ...rest } = $props();
</script>

<Alert {...rest} dismissible>
  {@render children?.()}
</Alert>
