<script>
  import { fade as fadeTransition } from 'svelte/transition';
  import { classnames } from '../utils';

  let {
    class: className = '',
    children,
    closeAriaLabel = 'Close',
    closeClassName = '',
    color = 'success',
    dismissible = false,
    fade = true,
    heading = '',
    isOpen = true,
    toggle,
    theme,
    transition = { duration: fade ? 400 : 0 },
    heading: headingSnippet,
    ...rest
  } = $props();

  // Reactive state
  const showClose = $derived(dismissible || toggle);

  const handleToggle = $derived(toggle || (() => (isOpen = false)));

  const classes = $derived(
    classnames(className, 'alert', `alert-${color}`, {
      'alert-dismissible': showClose
    })
  );

  const closeClassNames = $derived(
    classnames('btn-close', closeClassName)
  );
</script>

{#if isOpen}
  <div
    {...rest}
    data-bs-theme={theme}
    transition:fadeTransition={transition}
    class={classes}
    role="alert"
  >
    {#if heading || headingSnippet}
      <h4 class="alert-heading">
        {heading}{@render headingSnippet?.()}
      </h4>
    {/if}
    {#if showClose}
      <button
        type="button"
        class={closeClassNames}
        aria-label={closeAriaLabel}
        onclick={handleToggle}
      />
    {/if}
    {#if children}
      {children}
    {:else}
      {@render children?.()}
    {/if}
  </div>
{/if}
