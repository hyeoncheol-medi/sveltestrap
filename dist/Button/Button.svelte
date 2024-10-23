<script>
  import { classnames } from '../utils';

  let {
    class: klass = '',  // className renamed to klass
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

  const ariaLabel = $derived(rest['aria-label']);

  const classes = $derived(
    classnames(
      klass,
      close ? 'btn-close' : 'btn',
      close || `btn${outline ? '-outline' : ''}-${color}`,
      size ? `btn-${size}` : false,
      block ? 'd-block w-100' : false,
      {
        active
      }
    )
  );

  const defaultAriaLabel = $derived(close ? 'Close' : null);
</script>

{#if href}
  <a
    {...rest}
    class={classes}
    class:disabled
    bind:this={inner}
    href={href}
    aria-label={ariaLabel || defaultAriaLabel}
  >
    {#if children}
      {children}
    {:else}
      {@render children?.()}
    {/if}
  </a>
{:else}
  <button
    {...rest}
    class={classes}
    disabled={disabled}
    bind:this={inner}
    value={value}
    aria-label={ariaLabel || defaultAriaLabel}
  >
    {#if children}
      {children}
    {:else}
      {@render children?.()}
    {/if}
  </button>
{/if}
