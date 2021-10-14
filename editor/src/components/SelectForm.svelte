<script>
    import { store_dictionary } from "../stores.js";
    
    export let element;

    let dictionary

    const unsubscribe = store_dictionary.subscribe((value) => {
        dictionary = value;
    });
</script>

<div>
    <div>{dictionary.select}:</div>
    <div class="form_element">
        <div class="form_left">{dictionary.table_name}</div>
        <div class="form_right">
            <input bind:value={element.select.table_name} type="text" />
        </div>
    </div>

    <div class="form_element">
        <div class="form_left">{dictionary.name_column}</div>
        <div class="form_right">
            <input bind:value={element.select.name_column} type="text" />
        </div>
    </div>

    <div class="form_element">
        <div class="form_left">{dictionary.id_column}</div>
        <div class="form_right">
            <input bind:value={element.select.id_column} type="text" />
        </div>
    </div>

    <div class="form_element">
        <div class="form_left">{dictionary.where_clause}</div>
        <div class="form_right">
            <input bind:value={element.select.where_clause} type="text" />
        </div>
    </div>
</div>

<style>
    .form_element {
        display: flex;
    }
    .form_left {
        width: 170px;
    }
    .form_right {
        flex-grow: 1;
    }
</style>
