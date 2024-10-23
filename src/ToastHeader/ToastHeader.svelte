<script>
  import { classnames } from '../utils';
  import { Button } from '../Button';

  let {
    class: klass = '',
    icon = null,
    toggle = null,
    closeAriaLabel = 'Close',
    children,
    icon: iconSlot,
    close: closeSlot
  } = $props();

  let classes = $derived(classnames(klass, 'toast-header'))

  let tagClassName = $derived(classnames('me-auto', { 'ms-2': icon !== null }));
</script>

<div {...rest} class={classes}>
  {#if icon}
    <svg
      class={`rounded text-${icon}`}
      width="20"
      height="20"
      xmlns="http://www.w3.org/2000/svg"
      preserveAspectRatio="xMidYMid slice"
      focusable="false"
      role="img"
    >
      <rect fill="currentColor" width="100%" height="100%" />
    </svg>
  {:else}
    {@render iconSlot?.()}
  {/if}
  <strong class={tagClassName}>
    {@render children?.()}
  </strong>
  {#if toggle}
    {#if closeSlot}
      {@render closeSlot?.()}
    {:else}
      <Button
        close
        onclick={toggle}
        aria-label={closeAriaLabel}
      />
    {/if}
  {/if}
</div>
