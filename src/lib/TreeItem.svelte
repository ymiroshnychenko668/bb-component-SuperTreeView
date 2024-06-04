<script>
    import { getContext } from "svelte";
    
    
    export let selectable
    export let selected = false
    export let open = false
    export let href = false
    
    export let title
    export let icon 
    export let size
    export let key = ""
    export let children = [];
    export let nodeIDColumn;
    export let itemRelColumn;
    export let nodeValueColumn;
    export let onExpand
    export let onClick
    export let onCollapse
    // export let standalone = true
    // export let quiet = false
    // export let width = "250px"
    // export let selectionHandler

    const selectedItems = getContext("selectedItems")
    function handleExpanderClick(event){
      if (children?.length>0){
        if(onExpand){
         if(!open){
            open = true;
            onExpand({nodeKey :key, nodeValue :title})

          }else{
            open = false;
            onCollapse({nodeKey :key, nodeValue :title})
          }
      }
      }
      
    
    }
      
    function handleClick (event) { 
     
      if (onClick) {  
        onClick({ 'nodeKey' : key, 'nodeValue': title});
      }
    }
    let iconSize = "ri-sm"
    $: nodeType = (children?.lenght==0 )? "Item" : "Node"
  </script>
  <!-- svelte-ignore a11y-click-events-have-key-events -->
    <li class:is-selected={selected} class:is-open={open} class="spectrum-TreeView-item">
    <div  class="spectrum-TreeView-itemLink"> 
      {#if children?.length>0}
        <span {href} on:click={handleExpanderClick} >
          <svg class="spectrum-Icon spectrum-UIIcon-ChevronRight100 spectrum-TreeView-itemIndicator" focusable="false" aria-hidden="true">
            <use xlink:href="#spectrum-css-icon-Chevron100" />
          </svg>
        </span>
      {/if}
      <span class="spectrum-TreeView-itemLink" {href}  on:click={handleClick} >       
        {#if icon}
          <i class="{icon} {iconSize}" />
        {/if} 
        {title} 
      </span>
    </div>
    {#if children?.length>0}
      <ul class="spectrum-TreeView spectrum-TreeView--size{size}" >         
       {#each children as item}
          <svelte:self {selectable} key={item[nodeIDColumn]} parentKey={key} onClick={onClick} 
            title={item[nodeValueColumn]} icon={icon}  children={item[itemRelColumn]} 
            nodeIDColumn={nodeIDColumn} nodeValueColumn={nodeValueColumn} itemRelColumn= {itemRelColumn}/>           
        {/each}
      </ul>
    {/if}  
    </li>  
  
 
  <style>
    i {
      margin-right: 0.25rem;
    }
    .spectrum-TreeView-itemLabel {
      display: flex;
      align-items: center;
    }
  </style>