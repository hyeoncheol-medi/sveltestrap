<script>
  import { run, createBubbler } from 'svelte/legacy';

  const bubble = createBubbler();
  import { FormCheck } from '../FormCheck';
  import { FormFeedback } from '../FormFeedback';
  import { classnames } from '../utils';

  /**
   * Additional CSS classes for the input.
   * @type {string}
   * @default ''
   */
  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {string | undefined} [bsSize] - Bootstrap size to apply to the input.
   * @property {boolean} [checked] - Indicates whether the input should be checked
Used with type "checkbox", "radio", and "switch".
   * @property {string | undefined} [color] - Color selection of the input
Used with type "color"
   * @property {boolean | undefined} [disabled] - Determines whether the input is disabled.
   * @property {string | Array} [feedback] - Feedback message for the Input.
   * @property {Array | undefined} [files] - Array of files for file names for input.
Used with type "file"
   * @property {string | undefined} [group] - Group identifier for the Input.
Used with type "checkbox", "radio", and "switch".
   * @property {string | undefined} [inner] - Inner content for the Input.
   * @property {boolean} [invalid] - Indicates whether the input is invalid.
   * @property {string | undefined} [label] - Label for the input.
Used with type "checkbox", "radio", "switch".
   * @property {string | number | undefined} [max] - Maximum value for the Input.
Used with type "date", "datetime", "datetime-local", "month", "number", "time", "range", and "week"
   * @property {string | number | undefined} [min] - Minimum value for the Input.
Used with type "date", "datetime", "datetime-local", "month", "number", "time", "range", and "week"
   * @property {boolean | undefined} [multiple] - Indicates whether the Input allows multiple selections.
Used with type "email" and "file".
   * @property {string} [name] - Name attribute for the Input.
   * @property {string} [placeholder] - Placeholder text for the input.
   * @property {boolean} [plaintext] - Indicates whether the input should be rendered in plaintext mode.
   * @property {boolean | undefined} [readonly] - Indicates whether the input is read-only.
   * @property {boolean} [reverse] - Indicates whether the input is reversed.
   * @property {string | undefined} [size] - Determines the size of the input.
   * @property {string | undefined} [theme] - Theme name override to apply to the input.
   * @property {string} [type] - Determines the type of input.
   * @property {boolean} [valid] - Indicates whether the input is valid.
   * @property {string | undefined} [value] - Value of the input.
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    bsSize = $bindable(undefined),
    checked = $bindable(false),
    color = undefined,
    disabled = undefined,
    feedback = undefined,
    files = $bindable(undefined),
    group = $bindable(undefined),
    inner = $bindable(undefined),
    invalid = false,
    label = undefined,
    max = undefined,
    min = undefined,
    multiple = undefined,
    name = '',
    placeholder = '',
    plaintext = false,
    readonly = undefined,
    reverse = false,
    size = $bindable(undefined),
    theme = undefined,
    type = 'text',
    valid = false,
    value = $bindable(undefined),
    children,
    ...rest
  } = $props();

  let classes = $state();
  let tag = $state();

  run(() => {
    const isNotaNumber = new RegExp('\\D', 'g');

    let isBtn = false;
    let formControlClass = 'form-control';
    tag = 'input';

    switch (type) {
      case 'color':
        formControlClass = `form-control form-control-color`;
        break;
      case 'range':
        formControlClass = 'form-range';
        break;
      case 'select':
        formControlClass = `form-select`;
        tag = 'select';
        break;
      case 'textarea':
        tag = 'textarea';
        break;
      case 'button':
      case 'reset':
      case 'submit':
        formControlClass = `btn btn-${color || 'secondary'}`;
        isBtn = true;
        break;
      case 'hidden':
      case 'image':
        formControlClass = undefined;
        break;
      default:
        formControlClass = 'form-control';
        tag = 'input';
    }

    if (plaintext) {
      formControlClass = `${formControlClass}-plaintext`;
      tag = 'input';
    }

    if (size && isNotaNumber.test(size)) {
      console.warn('Please use the prop "bsSize" instead of the "size" to bootstrap\'s input sizing.');
      bsSize = size;
      size = undefined;
    }

    classes = classnames(className, formControlClass, {
      'is-invalid': invalid,
      'is-valid': valid,
      [`form-control-${bsSize}`]: bsSize && !isBtn && tag !== 'select',
      [`form-select-${bsSize}`]: bsSize && tag === 'select',
      [`btn-${bsSize}`]: bsSize && isBtn
    });
  });
</script>

{#if tag === 'input'}
  {#if type === 'text' || type === 'password' || type === 'search' || type === 'tel' || type === 'url'}
    <input
      {...rest}
      {...{ type }}
      data-bs-theme={theme}
      class={classes}
      bind:value
      bind:this={inner}
      onblur={bubble('blur')}
      onchange={bubble('change')}
      onclick={bubble('click')}
      onfocus={bubble('focus')}
      oninput={bubble('input')}
      onkeydown={bubble('keydown')}
      onkeypress={bubble('keypress')}
      onkeyup={bubble('keyup')}
      onmousedown={bubble('mousedown')}
      onmouseup={bubble('mouseup')}
      {disabled}
      {name}
      {placeholder}
      {readonly}
      {size}
    />
  {:else if type === 'color'}
    <input
      {...rest}
      data-bs-theme={theme}
      class={classes}
      type="color"
      bind:value
      bind:this={inner}
      onblur={bubble('blur')}
      onchange={bubble('change')}
      onclick={bubble('click')}
      onfocus={bubble('focus')}
      oninput={bubble('input')}
      onkeydown={bubble('keydown')}
      onkeypress={bubble('keypress')}
      onkeyup={bubble('keyup')}
      onmousedown={bubble('mousedown')}
      onmouseup={bubble('mouseup')}
      {disabled}
      {name}
      {placeholder}
      {readonly}
    />
  {:else if type === 'email'}
    <input
      {...rest}
      data-bs-theme={theme}
      class={classes}
      type="email"
      bind:value
      bind:this={inner}
      onblur={bubble('blur')}
      onchange={bubble('change')}
      onclick={bubble('click')}
      onfocus={bubble('focus')}
      oninput={bubble('input')}
      onkeydown={bubble('keydown')}
      onkeypress={bubble('keypress')}
      onkeyup={bubble('keyup')}
      onmousedown={bubble('mousedown')}
      onmouseup={bubble('mouseup')}
      {disabled}
      {multiple}
      {name}
      {placeholder}
      {readonly}
      {size}
    />
  {:else if type === 'file'}
    <input
      {...rest}
      data-bs-theme={theme}
      class={classes}
      type="file"
      bind:files
      bind:value
      bind:this={inner}
      onblur={bubble('blur')}
      onchange={bubble('change')}
      onclick={bubble('click')}
      onfocus={bubble('focus')}
      oninput={bubble('input')}
      onkeydown={bubble('keydown')}
      onkeypress={bubble('keypress')}
      onkeyup={bubble('keyup')}
      onmousedown={bubble('mousedown')}
      onmouseup={bubble('mouseup')}
      {disabled}
      {invalid}
      {multiple}
      {name}
      {placeholder}
      {readonly}
      {valid}
    />
  {:else if type === 'checkbox' || type === 'radio' || type === 'switch'}
    <FormCheck
      {...rest}
      data-bs-theme={theme}
      class={className}
      size={bsSize}
      {type}
      bind:checked
      bind:inner
      bind:group
      bind:value
      on:blur
      on:change
      on:focus
      on:input
      on:keydown
      on:keypress
      on:keyup
      on:mousedown
      on:mouseup
      {disabled}
      {invalid}
      {label}
      {name}
      {placeholder}
      {reverse}
      {readonly}
      {valid}
    />
  {:else if type === 'date' || type === 'datetime' || type === 'datetime-local' || type === 'month' || type === 'number' || type === 'time' || type === 'range' || type === 'week'}
    <input
      {...rest}
      {...{ type }}
      data-bs-theme={theme}
      class={classes}
      bind:value
      bind:this={inner}
      onblur={bubble('blur')}
      onchange={bubble('change')}
      onclick={bubble('click')}
      onfocus={bubble('focus')}
      oninput={bubble('input')}
      onkeydown={bubble('keydown')}
      onkeypress={bubble('keypress')}
      onkeyup={bubble('keyup')}
      onmousedown={bubble('mousedown')}
      onmouseup={bubble('mouseup')}
      {disabled}
      {max}
      {min}
      {name}
      {placeholder}
      {readonly}
    />
  {:else}
    <input
      {...rest}
      data-bs-theme={theme}
      class={classes}
      {...{ type }}
      bind:this={inner}
      bind:value
      onblur={bubble('blur')}
      onchange={bubble('change')}
      onclick={bubble('click')}
      onfocus={bubble('focus')}
      oninput={bubble('input')}
      onkeydown={bubble('keydown')}
      onkeypress={bubble('keypress')}
      onkeyup={bubble('keyup')}
      onmousedown={bubble('mousedown')}
      onmouseup={bubble('mouseup')}
      {name}
      {disabled}
      {placeholder}
      {readonly}
    />
  {/if}
{:else if tag === 'textarea'}
  <textarea
    {...rest}
    data-bs-theme={theme}
    class={classes}
    bind:value
    bind:this={inner}
    onblur={bubble('blur')}
    onchange={bubble('change')}
    onclick={bubble('click')}
    onfocus={bubble('focus')}
    oninput={bubble('input')}
    onkeydown={bubble('keydown')}
    onkeypress={bubble('keypress')}
    onkeyup={bubble('keyup')}
    onmousedown={bubble('mousedown')}
    onmouseup={bubble('mouseup')}
    {disabled}
    {name}
    {placeholder}
    {readonly}
></textarea>
{:else if tag === 'select' && !multiple}
  <select
    {...rest}
    data-bs-theme={theme}
    class={classes}
    bind:value
    bind:this={inner}
    onblur={bubble('blur')}
    onclick={bubble('click')}
    onchange={bubble('change')}
    onfocus={bubble('focus')}
    oninput={bubble('input')}
    {name}
    {disabled}
    {readonly}
  >
    {@render children?.()}
  </select>
{/if}

{#if feedback}
  {#if Array.isArray(feedback)}
    {#each feedback as msg}
      <FormFeedback {valid}>{msg}</FormFeedback>
    {/each}
  {:else}
    <FormFeedback {valid}>{feedback}</FormFeedback>
  {/if}
{/if}
