<script>
  import { onMount } from 'svelte';

  export let element;
  let result = null

  const table_name = element.select.table_name;
  const name_column = element.select.name_column;
  const id_column = element.select.id_column;
  const where_clause = element.select.where_clause

  const query =
    '{  selectList(tableName:"' + table_name + '", nameColumn:"' + name_column + '", idColumn: "' + id_column +'"whereClause: "' + where_clause + '") {    id    name }}';

  onMount(async () => {
    const res = await fetch(`http://192.168.20.35:8000/?query=` + query);
    result = await res.json();
  });

  let selected;
</script>

<select bind:value={element.value}>
  {#if result}
  {#each result.data.selectList as option}
  <option>{option.name}</option>
  {/each}
  {/if}
</select>
<br />
