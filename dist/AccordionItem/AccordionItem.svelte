<script>
  import { AccordionHeader } from '../AccordionHeader';
  import { Collapse } from '../Collapse';
  import { classnames } from '../utils';

  let {
    class: klass = '',
    header = '',
    active = false,
    children,
    headerContent,
    onintrostart,
    onintroend,
    onoutrostart,
    onoutroend,
    ontoggle
  } = $props();

  const { stayOpen, toggle, open } = getContext('accordion');

  let accordionRef = $state();
  let isOpen = $derived(stayOpen ? active : $open === accordionRef);
  let classes = $derived(classnames(klass, 'accordion-item'));

  $effect(() => {
    if (active) {
      toggle(accordionRef);
    }
  });

  function onToggle() {
    if (stayOpen) {
      active = !active;
    }

    toggle(accordionRef);
    ontoggle?.(!isOpen);
  }
</script>

<div class={classes} bind:this={accordionRef}>
  <AccordionHeader
    onclick={onToggle}
    class={!isOpen && 'collapsed'}
  >
    {@render headerContent?.() ?? header}
  </AccordionHeader>
  <Collapse
    isOpen={isOpen}
    class="accordion-collapse"
    {onintrostart}
    {onintroend}
    {onoutrostart}
    {onoutroend}
  >
    <div class="accordion-body">
      {@render children()}
    </div>
  </Collapse>
</div>
