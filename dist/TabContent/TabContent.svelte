<script>
  import { createEventDispatcher, setContext } from 'svelte';
  import { writable } from 'svelte/store';
  import { classnames } from '../utils';
  import { TabHeader } from '../TabHeader';

  const dispatch = createEventDispatcher();

  
  /**
   * @typedef {Object} Props
   * @property {string} [class]
   * @property {boolean} [pills]
   * @property {boolean} [vertical]
   * @property {import('svelte').Snippet} [children]
   */

  /** @type {Props & { [key: string]: any }} */
  let {
    class: className = '',
    pills = false,
    vertical = false,
    children,
    ...rest
  } = $props();

  const activeTabId = writable();
  setContext('tabContent', {
    activeTabId,
    setActiveTab: (tabId) => {
      activeTabId.set(tabId);
      dispatch('tab', tabId);
    }
  });

  let classes = $derived(classnames('tab-content', className, {
    'd-flex align-items-start': vertical
  }));
</script>

<div {...rest} class={classes}>
  <TabHeader class={classnames({ 'me-3': vertical })} {pills} tabs={!pills} {vertical}>
    {@render children?.()}
  </TabHeader>
  {@render children?.()}
</div>
