<script>
  import { createEventDispatcher } from 'svelte'

  const dispatch = createEventDispatcher()
  export let caption = null
  export let columns
  export let rows
  export let alignRight = []
  export let mapLinks = null

  function textAlign(idx) {
    if (alignRight.indexOf(idx + 1) !== -1) {
      return 'text-right'
    }
    return ''
  }

  const mapLinkClick = (idx) => () => {
    if (mapLinks) dispatch('mapLink', idx)
  }

</script>

<style>
.mapLink {
  @apply  cursor-pointer;
}
.mapLink:hover {
  @apply bg-blue-200;
}

@media screen and (max-width: 639px) {
  .table-component thead { display: none; }

  .table-component tr {
    display: block;
    margin: 0.75rem 0;
    box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06);
    border-radius: .25rem;
    /* @apply block my-3 shadow rounded; */
  }

  .table-component td {
    display: block;
    text-align: right;
    /* @apply block text-right; */
  }

  .table-component td:before {
    content: attr(data-label);
    text-transform: uppercase;
    float: left;
    font-weight: bold;
    /* @apply uppercase float-left font-bold; */
  }
}
</style>

<div class="w-full pt-10 pb-5">
  <table class="table-component table-auto w-full">
    {#if caption}
    <caption class="text-left text-xl font-bold uppercase">{@html caption }</caption>
    {/if}

    {#if columns}
    <thead>
      <tr class="border-b-2 border-blue-500">
        {#each columns as column, i}
        <th class="uppercase text-left px-4 py-2 text-gray-600 {textAlign(i)}">{column}</th>
        {/each}
      </tr>
    </thead>
    {/if}

    {#if rows}
    <tbody>
      {#each rows as row, idx}
        <tr class="transition-all" class:mapLink="{mapLinks}" on:click={mapLinkClick(mapLinks[idx])}>
          {#each row as elem, i}
            <td data-label="{ columns[i] }" class="px-4 py-2 {textAlign(i)}">{@html elem }</td>
          {/each}
        </tr>
      {/each}
    </tbody>
    {/if}
  </table>
</div>
