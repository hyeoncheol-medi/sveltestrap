<script>
  import { classnames } from '../utils';

  let {
    class: className = '',
    active = false,
    block = false,
    children = '',
    close = false,
    color = 'secondary',
    disabled = false,
    href = '',
    inner = undefined,
    outline = false,
    size = '',
    value = '',
    ...rest
  } = $props();

  $: classes = classnames(
    className,
    close ? 'btn-close' : 'btn',
    close || `btn${outline ? '-outline' : ''}-${color}`,
    size ? `btn-${size}` : false,
    block ? 'd-block w-100' : false,
    {
      active
    }
  );

  $: ariaLabel = rest['aria-label'];
  $: defaultAriaLabel = close ? 'Close' : null;
</script>

{#if href}
<a
    {...rest}
    class={classes}
    class:disabled
    bind:this={inner}
    {href}
    onclick
    onfocus
    onblur
    aria-label={ariaLabel || defaultAriaLabel}
  >
    {#if children}
      {children}
    {:else}
      {@render rest.children?.()}
    {/if}
  </a>
{:else}
  <button
    {...rest}
    class={classes}
    {disabled}
    bind:this={inner}
    {value}
    onclick
    onfocus
    onblur
    aria-label={ariaLabel || defaultAriaLabel}
  >
    {#if children}
      {children}
    {:else}
      {@render rest.children?.()}
    {/if}
  </button>
{/if}
