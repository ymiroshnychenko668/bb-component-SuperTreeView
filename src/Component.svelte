<script>
//  import "../node_modules/@spectrum-css/treeview/dist/index-vars.css"

  import Welcome from "./lib/Welcome.svelte";
  import { getContext } from "svelte"

  const { styleable} = getContext("sdk")    

  const component = getContext("component")


  import TreeItem from "./lib/TreeItem.svelte";

  const loading = getContext("loading")

  
  export let dataProvider 
  export let nodeIDColumn
  export let itemRelColumn


  export let width = '100%'

  $: rows = $loading ? new Array(dataProvider?.limit > 20 ? 20 : dataProvider?.limit).fill({}) : dataProvider?.rows
  $: settings =   {
    itemRelColumn: itemRelColumn,
    nodeIDColumn: nodeIDColumn,
    width:width
  }
</script>
<style>
  @import './lib/ionicon.css';
</style>
<!-- svelte-ignore a11y-click-events-have-key-events -->

  {#key settings}
    {#if rows}
    <nav class="tree-nav" use:styleable={$component.styles} style="width: {width};">
      {#each rows as row }
        <TreeItem nodeIDColumn={nodeIDColumn} itemRelColumn={itemRelColumn} node={row}>
          <slot></slot>
        </TreeItem>
      {/each}
      </nav>

    {:else}
      <Welcome> <slot /> </Welcome>
    {/if}
  {/key}

 