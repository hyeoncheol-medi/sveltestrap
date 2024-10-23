<script>
  import { classnames } from '../utils';

  let {
    class: klass = '',
    check = false,
    disabled = false,
    floating = false,
    inline = false,
    label = '',
    row = false,
    spacing = 'mb-3',
    tag = null,
    children,
    labelContent,
    ...rest
  } = $props();

  let classes = $derived(classnames(klass, spacing, {
    row,
    'form-check': check,
    'form-check-inline': check && inline,
    'form-floating': floating,
    disabled: check && disabled
  }));
</script>

{#if tag === 'fieldset'}
  <fieldset {...rest} class={classes}>
    {@render children?.()}
    {#if label || labelContent}
      <!-- svelte-ignore a11y-label-has-associated-control -->
      <label>
        {label}
        {@render labelContent?.()}
      </label>
    {/if}
  </fieldset>
{:else}
  <div {...rest} class={classes}>
    {@render children?.()}
    {#if label || labelContent}
      <!-- svelte-ignore a11y-label-has-associated-control -->
      <label>
        {label}
        {@render labelContent?.()}
      </label>
    {/if}
  </div>
{/if}
