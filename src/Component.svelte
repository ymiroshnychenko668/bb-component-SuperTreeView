<script>
//  import "../node_modules/@spectrum-css/treeview/dist/index-vars.css"
  
  import Welcome from "./lib/Welcome.svelte";
  import { getContext } from "svelte"
  import { children } from "svelte/internal";

  const { styleable,Provider ,ContextScopes} = getContext("sdk")
  const component = getContext("component")
  const loading = getContext("loading")

  export let dataProvider 
  export let nodeIDColumn
  export let nodeValueColumn
  export let itemRelColumn
  export let nodeIcon
  
  export let onClick
  export let onExpand
  export let onCollapse

  export let selected = false
  export let open = false
  export let href = false
    

  export let quiet = false
  export let standalone = true
  export let selectable = false
  export let size
  export let width = "250px"
  export let scope = ContextScopes.Local



  export let itemRel
  let iconSize = "ri-sm"

    // $: nodeType = (children?.lenght==0 )? "Item" : "Node"
  // const selectedItems = writable([])
  // setContext("selectedItems", selectedItems)

  // If the parent DataProvider is loading, fill the rows array with a number of empty objects 
  // corresponding to the DataProvider's page size; 
  // this allows skeleton loader components to be rendered further down the tree.
  function handleExpanderClick(event){
      if (children?.length>0){
         if(!open){
            open = true;
            if(onExpand)
              onExpand({nodeKey :node[nodeIDColumn], nodeValue : node[nodeValueColumn]})
          }else{
            open = false;
            if(onExpand)
              onCollapse({nodeKey :node[nodeIDColumn], nodeValue : node[nodeValueColumn]})
          }

      }
    }
    function handleClick (event) { 
      if (onClick) {  
        onClick({nodeKey :node[nodeIDColumn], nodeValue : node[nodeValueColumn]})
      }
    }


    
  $: rows = $loading ? new Array(dataProvider?.limit > 20 ? 20 : dataProvider?.limit).fill({}) : dataProvider?.rows
  $: node = itemRel ? itemRel : rows?.length > 0 ? rows[0] : undefined
  $: settings =   {
    loading: $loading,
    node: node
  }


  
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->

  {#key settings}
    {#if node}
      <ul class:spectrum-TreeView--standalone={standalone} class:spectrum-TreeView--quiet={quiet} class="spectrum-TreeView spectrum-TreeView--size{size}">
         <li class:is-selected={selected} class:is-open={open} class="spectrum-TreeView-item">
          <div  class="spectrum-TreeView-itemLink"> 
              {#if node[itemRelColumn]?.length>0}
                <span {href} on:click={handleExpanderClick} >
                  <svg class="spectrum-Icon spectrum-UIIcon-ChevronRight100 spectrum-TreeView-itemIndicator" focusable="false" aria-hidden="true">
                    <use xlink:href="#spectrum-css-icon-Chevron100" />
                  </svg>
                </span>
              {/if}
              <div use:styleable={$component.styles} class="spectrum-TreeView-itemLink" {href}>
                <Provider data={{ ...node }} {scope}>
                  <slot />  
                </Provider> 
              </div>
            </div>
            {#if node[itemRelColumn]?.length>0}
               {#each node[itemRelColumn] as item, index}
                  <svelte:self {selectable} itemRel={item} onClick={onClick} onCollapse={onCollapse} onExpand={onExpand} icon={nodeIcon}  
                    nodeIDColumn={nodeIDColumn} nodeValueColumn={nodeValueColumn} itemRelColumn= {itemRelColumn}> 
                    <slot></slot>
                  </svelte:self>           
                {/each} 
          {/if}  
          </li>   
    </ul>
    {:else}
      <Welcome> <slot /> </Welcome>
    {/if}
    {/key}


<style>
    i {
      margin-right: 0.25rem;
    }
    .spectrum-TreeView-itemLink{
      
    }
    .spectrum-TreeView-itemLabel {
      display: flex;
      align-items: center;
    }
  .wrapper {
    display: flex;
    flex-direction: row;
  }
  .sideView {
    flex-grow: 1 ;
  }
</style>