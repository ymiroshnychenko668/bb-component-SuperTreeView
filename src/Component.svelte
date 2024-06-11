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


  export let onExpand
  export let onCollapse
  export let onClick

  export let width

  $: rows = $loading ? new Array(dataProvider?.limit > 20 ? 20 : dataProvider?.limit).fill({}) : dataProvider?.rows
  $: settings =   {
    onExpand:onExpand,
    onCollapse:onCollapse,
    itemRelColumn: itemRelColumn,
    nodeIDColumn: nodeIDColumn
  }
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->

  {#key settings}
    {#if rows}
    <nav class="tree-nav" use:styleable={$component.styles}>
      {#each rows as row }
        <TreeItem nodeIDColumn={nodeIDColumn} itemRelColumn={itemRelColumn} onExpand={onExpand} onClick={onClick}
         onCollapse={onCollapse} node={row}>
          <slot></slot>
        </TreeItem>
      {/each}
      </nav>

    {:else}
      <Welcome> <slot /> </Welcome>
    {/if}
  {/key}

 