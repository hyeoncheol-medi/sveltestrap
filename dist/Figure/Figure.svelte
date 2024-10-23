<script>
  import { setContext } from 'svelte';
  import { classnames } from '../utils';

  setContext('figure', true);

  let {
    class: klass = '',
    alt,
    caption,
    children,
    captionSnippet
  } = $props();

  let classes = $derived(classnames('figure', klass));
</script>

<img {alt} class={classes} />

<figure class={classes}>
  {@render children?.()}
  {#if caption || captionSnippet}
    <figcaption class="figure-caption">
      {caption}{@render captionSnippet?.()}
    </figcaption>
  {/if}
</figure>
