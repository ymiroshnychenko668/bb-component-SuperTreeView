<script>
//  import "../node_modules/@spectrum-css/treeview/dist/index-vars.css"

  import Welcome from "./lib/Welcome.svelte";
  import { getContext } from "svelte"

  const { styleable} = getContext("sdk")    

  const component = getContext("component")


  import TreeItem from "./lib/TreeItem.svelte";
    import { listen } from "svelte/internal";

  const loading = getContext("loading")

  
  export let dataProvider 
  export let nodeIDColumn
  export let itemRelColumn
  export let itemValueColumn


  export let onExpand
  export let onCollapse
  export let onClick

  export let selectable = false
  
  export let detached = true
  export let quiet = true


  export let size
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
    <nav  class:spectrum-TreeView--detached={detached} class:spectrum-TreeView--quiet={quiet} class="spectrum-TreeView spectrum-TreeView--size{size}" 
    style="width: {width};" use:styleable={$component.styles}>
      {#each rows as row }
        <TreeItem nodeIDColumn={nodeIDColumn} itemRelColumn={itemRelColumn} onExpand={onExpand} onClick={onClick}
         onCollapse={onCollapse} node={row} detached={detached} quiet={quiet} itemValueColumn={itemValueColumn} >
          <slot></slot>
        </TreeItem>
      {/each}
      </nav>

    {:else}
      <Welcome> <slot /> </Welcome>
    {/if}
  {/key}

  <style>
    /*
 Only custom marker for summary/details
 For cross browser compatible styling hide Firefox's marker by setting summary { display: block }
 and Chrome and Safari's marker by setting ::-webkit-details-marker {display: none;}
*/

.tree-nav__item {
  display: block;
  white-space: nowrap;
  color: #ccc;
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
  padding-left: 2.4rem;
}
.tree-nav__item.is-expandable > .tree-nav__item-title::before {
  position: absolute;
  will-change: transform;
  transition: transform 300ms ease;
  font-family: "ionicons";
  color: #fff;
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
  color: #fff;
  font-size: 1.5rem;
  line-height: 3.2rem;
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