<script>
  import { getContext, onMount } from 'svelte';
  import { NavItem } from '../NavItem';
  import { NavLink } from '../NavLink';
  import { classnames } from '../utils';

  let {
    class: klass = '',
    active = false,
    disabled = false,
    tab = undefined,
    tabId = undefined,
    tab: tabSnippet,
    children
  } = $props();

  const tabs = getContext('tabs');
  const { activeTabId, setActiveTab } = getContext('tabContent');

  onMount(() => {
    if (active) setActiveTab(tabId);
  });

  let tabOpen = $state(active);

  $effect(() => {
    if ($activeTabId !== undefined) {
      tabOpen = $activeTabId === tabId;
    }
  });

  const classes = $derived(classnames('tab-pane', klass, {
    active: tabOpen,
    show: tabOpen
  }));
</script>

{#if tabs}
  <NavItem>
    <NavLink
      active={tabOpen}
      disabled={disabled}
      onclick={() => setActiveTab(tabId)}
    >
      {#if tab}{tab}{/if}
      {@render tabSnippet?.()}
    </NavLink>
  </NavItem>
{:else}
  <div {...rest} class={classes}>
    {@render children?.()}
  </div>
{/if}
