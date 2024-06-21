<script>
    import { getContext } from "svelte"
    const { styleable,Provider ,ContextScopes} = getContext("sdk")    
    const component = getContext("component")
   
   
    export let scope = ContextScopes.Local
    export let nodeIDColumn
    export let itemRelColumn  
    export let node
        
    $: isExpandable = node[itemRelColumn]?.length>0


</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-missing-attribute -->

{#if node}
<details class="tree-nav__item" class:is-expandable={isExpandable} >

 <summary class="tree-nav__item-title">
            <div class="tree-nav__item">
            <Provider data={{ ...node }} {scope} >
                
                <slot />  
                
            </Provider>
            </div>
        </summary>

      {#if node[itemRelColumn]?.length>0}
            {#each node[itemRelColumn] as item, index}
              <svelte:self  node={item} nodeIDColumn={nodeIDColumn} itemRelColumn={itemRelColumn}> 
                <slot></slot>
               </svelte:self>           
             {/each} 
      
    {/if}  
</details>        
{/if}

<style>
    @import './ionicon.css';


    /*
 Only custom marker for summary/details
 For cross browser compatible styling hide Firefox's marker by setting summary { display: block }
 and Chrome and Safari's marker by setting ::-webkit-details-marker {display: none;}
*/
/*
 Only custom marker for summary/details
 For cross browser compatible styling hide Firefox's marker by setting summary { display: block }
 and Chrome and Safari's marker by setting ::-webkit-details-marker {display: none;}
*/
summary {
  display: block;
  cursor: pointer;
  outline: 1; 
}

summary::-webkit-details-marker {
    display: none;
  }



.tree-nav__item {
  display: block;
  white-space: nowrap;
  position: relative;
}
.tree-nav__item.is-expandable::before {
  border-left: 1px solid #333;
  content: "";
  height: 100%;
  left: 0.8rem;
  position: absolute;
  top: 2.4rem;
  height: calc(100% - 2.4rem);
}
.tree-nav__item .tree-nav__item {
  margin-left: 2.4rem;
}
.tree-nav__item.is-expandable[open] > .tree-nav__item-title::before {
  font-family: "ionicons";
  transform: rotate(90deg);
}
.tree-nav__item.is-expandable > .tree-nav__item-title {
  /* padding-left: 2.4rem; */
}
.tree-nav__item.is-expandable > .tree-nav__item-title::before {
  position: absolute;
  will-change: transform;
  transition: transform 300ms ease;
  font-family: "ionicons";
  font-size: 1.1rem;
  content: "\f125";
  left: 0;
  display: inline-block;
  width: 1.6rem;
  text-align: center;
}

.tree-nav__item-title {
  cursor: pointer;
  display: block;
  outline: 0;
}
.tree-nav__item-title .icon {
  display: inline;
  padding-left: 1.6rem;
  margin-right: 0.8rem;
  color: #666;
  font-size: 1.4rem;
  position: relative;
}
.tree-nav__item-title .icon::before {
  top: 0;
  position: absolute;
  left: 0;
  display: inline-block;
  width: 1.6rem;
  text-align: center;
}

.tree-nav__item-title::-webkit-details-marker {
  display: none;
}
  </style>

