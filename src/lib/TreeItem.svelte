<script>
    import { children } from "svelte/internal";
    import { getContext } from "svelte"
    const { styleable,Provider ,ContextScopes} = getContext("sdk")    
    const component = getContext("component")
   
   
    export let scope = ContextScopes.Local
    export let nodeIDColumn
    export let itemRelColumn
    export let itemValueColumn

    export let onExpand
    export let onCollapse
    export let onClick
    export let node

    export let open = false
    export let selectable = false

    export let detached = false
    export let size = "S"
    export let width
    export let quiet = false
    
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

    function onClickHandler(e){
        alert('dasdasd');
        selected = true;
        if(onClick){
            onClick({nodeKey :node[nodeIDColumn], nodeValue : node[nodeValueColumn]});
        }
    }

    $: selected = selectable&&selected

</script>
<!-- svelte-ignore a11y-click-events-have-key-events -->
{#if node}
<li class="spectrum-TreeView-item spectrum-TreeView-item--size{size}"  class:is-open={open}  class:is-selected={selected}>
        <div class="spectrum-TreeView-itemLink" >
                {#if node[itemRelColumn]?.length>0}
                    <svg class="spectrum-Icon spectrum-UIIcon-ChevronRight100 spectrum-TreeView-itemIndicator" on:click={handleExpanderClick} focusable="false" aria-hidden="true">
                        <use xlink:href="#spectrum-css-icon-Chevron100" />
                    </svg>
                {/if}
        
            <!-- {#if itemValueColumn} 
                <span on:click={onClickHandler}>
                    {node[itemValueColumn]}
                </span>
            {/if} -->
            
            <Provider data={{ ...node }} {scope} >
                
                <slot />  
                
            </Provider>
        
        </div>
        
         
      {#if node[itemRelColumn]?.length>0}
        <ul class:spectrum-TreeView--detached={detached} class:spectrum-TreeView--quiet={quiet} 
            class="spectrum-TreeView spectrum-TreeView--size{size}" use:styleable={$component.styles} style="width: {width};" >
            {#each node[itemRelColumn] as item, index}
              <svelte:self {selectable} node={item} size={size} onCollapse={onCollapse} onExpand={onExpand} 
                nodeIDColumn={nodeIDColumn} itemRelColumn={itemRelColumn} nodeValueColumn={itemValueColumn}> 
                <slot></slot>
               </svelte:self>           
             {/each} 
         </ul>
    {/if}  
    </li>   
{/if}

