<script>
  import { classnames } from '../utils';

  let {
    class: className = '',
    checked = false,
    disabled = false,
    group = undefined,
    id = undefined,
    inline = false,
    inner = undefined,
    invalid = false,
    label = '',
    name = '',
    reverse = false,
    size = '',
    type = 'checkbox',
    valid = false,
    value = undefined,
    label: labelSnippet,
    ...rest
  } = $props();

  $: classes = classnames(className, 'form-check', {
    'form-check-reverse': reverse,
    'form-switch': type === 'switch',
    'form-check-inline': inline,
    [`form-control-${size}`]: size
  });

  $: inputClasses = classnames('form-check-input', {
    'is-invalid': invalid,
    'is-valid': valid
  });

  $: idFor = id || label;
</script>

<div class={classes}>
  {#if type === 'radio'}
    <input
      {...rest}
      class={inputClasses}
      id={idFor}
      type="radio"
      bind:group
      bind:this={inner}
      onblur
      onchange
      onfocus
      oninput
      {disabled}
      {name}
      {value}
    />
  {:else if type === 'switch'}
    <input
      {...rest}
      class={inputClasses}
      id={idFor}
      type="checkbox"
      bind:checked
      bind:this={inner}
      onblur
      onchange
      onfocus
      oninput
      {disabled}
      {name}
      {value}
    />
  {:else}
    <input
      {...rest}
      class={inputClasses}
      id={idFor}
      type="checkbox"
      bind:checked
      bind:this={inner}
      onblur
      onchange
      onfocus
      oninput
      {disabled}
      {name}
      {value}
    />
  {/if}
  {#if label}
    <label class="form-check-label" for={idFor}>
      {#if labelSnippet}
        {@render labelSnippet()}
      {:else}
        {label}
      {/if}
    </label>
  {/if}
</div>
