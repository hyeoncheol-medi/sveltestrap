<script>
  import { getContext } from 'svelte';

  let { class: className, footer, header, width, children, headerContent, footerContent } = $props();

  const colgroup = getContext('colgroup');
  const head = getContext('header');
  const foot = getContext('footer');
</script>

{#if colgroup}
  <col style="width: {width};" />
{:else if foot}
  <th {...$$restProps}>
    {#if footer}{footer}{/if}
    {@render footerContent?.()}
  </th>
{:else if head}
  <th {...$$restProps}>
    {#if header}{header}{/if}
    {@render headerContent?.()}
  </th>
{:else}
  <td class={className} {...$$restProps}>
    {@render children?.()}
  </td>
{/if}
