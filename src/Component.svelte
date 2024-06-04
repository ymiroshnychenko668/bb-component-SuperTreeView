<script>
//  import "../node_modules/@spectrum-css/treeview/dist/index-vars.css"
  
  import Welcome from "./lib/Welcome.svelte";
  import TreeItem from "./lib/TreeItem.svelte";
  import { getContext, setContext } from "svelte"
  import { writable } from "svelte/store";

  const { styleable } = getContext("sdk")
  const component = getContext("component")
  const loading = getContext("loading")

  export let onClick

  export let quiet = false
  export let standalone = true
  export let selectable = false
  export let title = ""
  export let size
  export let width = "250px"

  export let dataProvider 
  export let nodeIDColumn
  export let nodeValueColumn
  export let nodeIcon

  export let itemSource
  export let itemRelColumn
  export let onExpand;

  const selectedItems = writable([])

  setContext("selectedItems", selectedItems)

  // If the parent DataProvider is loading, fill the rows array with a number of empty objects 
  // corresponding to the DataProvider's page size; 
  // this allows skeleton loader components to be rendered further down the tree.
  $: rows = $loading ? new Array(dataProvider?.limit > 20 ? 20 : dataProvider?.limit).fill({}) : dataProvider?.rows
  $: _isColumnEnumerable = ( dataProvider?.schema ) ? ( dataProvider?.schema[itemRelColumn]?.type === "link" || dataProvider?.schema[itemRelColumn]?.type === "array" ) : false
  $: settings = { selectable : selectable, 
                  itemSource : itemSource, 
                  itemRelColumn: itemRelColumn }
</script>

<div use:styleable={$component.styles}>
  {#key settings}
    {#if rows?.length > 0}
      <ul class:spectrum-TreeView--standalone={standalone} class:spectrum-TreeView--quiet={quiet} class="spectrum-TreeView spectrum-TreeView--size{size}">
        {#if title !== "" }
          <div class="spectrum-TreeView-heading">
            <span class="spectrum-TreeView-itemLabel">{title.toUpperCase()}</span>
          </div>
        {/if}
        {#each rows as node}
          <TreeItem {selectable} onExpand={onExpand} onClick={onClick} key={node[nodeIDColumn]} icon={nodeIcon} title={node[nodeValueColumn] || "Set the Node Key & Label Columns"} {size} 
            nodeIDColumn={nodeIDColumn} nodeValueColumn ={nodeValueColumn} itemRelColumn= {itemRelColumn} children={node[itemRelColumn]} > 
            <span></span>
          </TreeItem>  
        {/each}
    </ul>
    {:else}
      <Welcome> <slot /> </Welcome>
    {/if}
  {/key}
</div>

<style>
  .wrapper {
    display: flex;
    flex-direction: row;
  }
  .sideView {
    flex-grow: 1 ;
  }
</style>