<script>
  import { flip } from 'svelte/animate';
  import { dndzone } from 'svelte-dnd-action';
  import { mennuItem } from '../store';


  export let columnItems;
  let i;
  let j;
  let item = {};

  const flipDurationMs = 300;
  function handleDndConsiderColumns(e) {
    columnItems = e.detail.items;
  }
  function handleDndFinalizeColumns(e) {
    columnItems = e.detail.items;
  }
  function handleDndConsiderCards(cid, e) {
    const colIdx = columnItems.findIndex(c => c.id === cid);
    columnItems[colIdx].items = e.detail.items;
    columnItems = [...columnItems];
  }
  function handleDndFinalizeCards(cid, e) {
    const colIdx = columnItems.findIndex(c => c.id === cid);
    columnItems[colIdx].items = e.detail.items;
    columnItems = [...columnItems];
  }
  function handleClick(index1, index2) {
    i = index1;
    j = index2;
    item = columnItems[i].items[j];
  }
  function handleChange() {
    columnItems = columnItems;
  }
</script>

<main class="main">
  <section
    class="board"
    use:dndzone="{{ items: columnItems, flipDurationMs, type: 'columns' }}"
    on:consider="{handleDndConsiderColumns}"
    on:finalize="{handleDndFinalizeColumns}">
    {#each columnItems as column, index1 (column.id)}
      <div class="column" animate:flip="{{ duration: flipDurationMs }}">
        <div class="column-title">{column.name}</div>
        <div
          class="column-content"
          use:dndzone="{{ items: column.items, flipDurationMs }}"
          on:consider="{e => handleDndConsiderCards(column.id, e)}"
          on:finalize="{e => handleDndFinalizeCards(column.id, e)}">
          {#each column.items as item, index2 (item.id)}
            <div
              class="card"
              animate:flip="{{ duration: flipDurationMs }}"
              on:click="{() => handleClick(index1, index2)}">
              {item.name}
            </div>
          {/each}
        </div>
      </div>
    {/each}
  </section>
  <section>
    <input bind:value="{item.name}" on:input="{handleChange}" type="text" />
  </section>
</main>

<style>
  .main {
    display: flex;
    height: 700px;
  }
  .board {
    height: 100vh;
    width: 50%;
    padding: 0.5em;
    margin-bottom: 40px;
    flex: 1;
  }
  .column {
    height: 100%;
    width: 250px;
    padding: 0.5em;
    margin: 1em;
    float: left;
    border: 1px solid #333333;
    /*Notice we make sure this container doesn't scroll so that the title stays on top and the dndzone inside is scrollable*/
    overflow-y: hidden;
  }
  .column-content {
    height: 100%;
    /* Notice that the scroll container needs to be the dndzone if you want dragging near the edge to trigger scrolling */
    overflow-y: scroll;
  }
  .column-title {
    margin-bottom: 1em;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .card {
    height: 15%;
    width: 100%;
    margin: 0.4em 0;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #dddddd;
    border: 1px solid #333333;
  }
</style>
