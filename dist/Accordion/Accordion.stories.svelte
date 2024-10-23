<script module>
  import Accordion from './Accordion.svelte';

  export const meta = {
    title: 'Stories/Accordion',
    component: Accordion,
    parameters: {
      controls: {
        exclude: /^(default)$/g
      }
    },
    argTypes: {
      class: {
        control: false,
        table: {
          disable: true
        }
      },
      flush: {
        control: 'boolean'
      },
      stayOpen: {
        control: 'boolean'
      },
      theme: {
        control: {
          type: 'select'
        },
        options: ['dark', 'light', 'auto'],
        description: 'The theme style to apply.',
        table: {
          type: {
            summary: 'string'
          },
          defaultValue: {
            summary: 'auto'
          }
        }
      },
      'default ': {
        description: 'This is the default content slot.',
        table: {
          category: 'slots',
          type: {
            summary: 'any'
          },
          defaultValue: {
            summary: 'empty'
          }
        }
      }
    },
    args: {
      class: '',
      flush: false,
      stayOpen: false,
      theme: null
    }
  };
</script>

<script>
  import { Story } from '@storybook/addon-svelte-csf';
  import { AccordionItem } from '@sveltestrap/sveltestrap';

  let id = $state(1);
  let open = $state(true);

  const basicSource = `<Accordion>
  <AccordionItem header="Home">Fallbrook</AccordionItem>
  <AccordionItem header="School">
    <a href="#home">Buena Vista Elementary</a>
  </AccordionItem>
  <AccordionItem header="Library">UCSB Library</AccordionItem>
</Accordion>`;
</script>

<Story name="Basic"  source={basicSource}>
  {#snippet children({ args })}
    {#key args}
      <Accordion {...args}>
        <AccordionItem header="Home">Fallbrook</AccordionItem>
        <AccordionItem header="School">
          <a href="#home">Buena Vista Elementary</a>
        </AccordionItem>
        <AccordionItem header="Library">UCSB Library</AccordionItem>
      </Accordion>
    {/key}
  {/snippet}
</Story>

<Story name="Events">
  <Accordion on:toggle={console.log}>
    <AccordionItem
      active
      header="Home"
      on:toggle={(e) => {
        id = 1;
        open = e.detail;
      }}
    >
      Fallbrook
    </AccordionItem>
    <AccordionItem
      header="School"
      on:toggle={(e) => {
        id = 2;
        open = e.detail;
      }}
    >
      <a href="#home">Buena Vista Elementary</a>
    </AccordionItem>
    <AccordionItem
      header="Library"
      on:toggle={(e) => {
        id = 3;
        open = e.detail;
      }}
    >
      UCSB Library
    </AccordionItem>
  </Accordion>
  <br />
  <code>Item #{id} is {open ? 'open' : 'closed'}</code>
</Story>

<Story name="Flush">
  <Accordion flush>
    <AccordionItem header="Home">Fallbrook</AccordionItem>
    <AccordionItem header="School">
      <a href="#home">Buena Vista Elementary</a>
    </AccordionItem>
    <AccordionItem header="Library">UCSB Library</AccordionItem>
  </Accordion>
</Story>

<Story name="Stay Open">
  <Accordion stayOpen>
    <AccordionItem header="Home">Fallbrook</AccordionItem>
    <AccordionItem header="School">
      <a href="#home">Buena Vista Elementary</a>
    </AccordionItem>
    <AccordionItem header="Library">UCSB Library</AccordionItem>
  </Accordion>
</Story>

<Story name="Slots">
  <Accordion>
    <AccordionItem active>
      {#snippet header()}
            <h4 class="m-0" >Home</h4>
          {/snippet}
      Fallbrook
    </AccordionItem>
    <AccordionItem>
      {#snippet header()}
            <h4 class="m-0" >School</h4>
          {/snippet}
      <a href="#home">Buena Vista Elementary</a>
    </AccordionItem>
    <AccordionItem>
      {#snippet header()}
            <h4 class="m-0" >Library</h4>
          {/snippet}
      UCSB Library
    </AccordionItem>
  </Accordion>
</Story>

<Story name="Theming">
  <Accordion theme="dark" class="mb-4">
    <AccordionItem active>
      {#snippet header()}
            <h4 class="m-0" >Home</h4>
          {/snippet}
      Fallbrook
    </AccordionItem>
    <AccordionItem>
      {#snippet header()}
            <h4 class="m-0" >School</h4>
          {/snippet}
      <a href="#home">Buena Vista Elementary</a>
    </AccordionItem>
    <AccordionItem>
      {#snippet header()}
            <h4 class="m-0" >Library</h4>
          {/snippet}
      UCSB Library
    </AccordionItem>
  </Accordion>

  <Accordion theme="light">
    <AccordionItem active>
      {#snippet header()}
            <h4 class="m-0" >Home</h4>
          {/snippet}
      Fallbrook
    </AccordionItem>
    <AccordionItem>
      {#snippet header()}
            <h4 class="m-0" >School</h4>
          {/snippet}
      <a href="#home">Buena Vista Elementary</a>
    </AccordionItem>
    <AccordionItem>
      {#snippet header()}
            <h4 class="m-0" >Library</h4>
          {/snippet}
      UCSB Library
    </AccordionItem>
  </Accordion>
</Story>
