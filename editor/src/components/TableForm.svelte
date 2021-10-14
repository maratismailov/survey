<script>
    import SelectForm from "./SelectForm.svelte";

    import {
        store_dictionary,
    } from "../stores.js";

    export let element;
    export let blank_element;
    let dictionary

    const unsubscribe = store_dictionary.subscribe((value) => {
        dictionary = value;
    });

    const add_column = () => {
        // element.fields.push(JSON.parse(JSON.stringify(blank_element.fields)))
        // console.log(blank_element.fields)
        // element.fields[0].push({...blank_element, select: {...blank_element.select}, fields: [...blank_element.fields]})
        element.fields[0].push(JSON.parse(JSON.stringify(blank_element)))
        console.log(element.fields)
        element = element
        // element.fields = [...element.fields, {...blank_element, select: {...blank_element.select}, fields: [...blank_element.fields]}];
    };
    console.log(element.fields)
</script>

<div>fields:</div>
<!-- {#if element.fields} -->
{#each element.fields[0] as element}
    <div class="form_element">
        <div class="form_left">element id</div>
        <div class="form_right">
            <input bind:value={element.id} type="text" />
        </div>
    </div>

    <div class="form_element">
        <div class="form_left">element name</div>
        <div class="form_right">
            <input bind:value={element.name} type="text" />
        </div>
    </div>

    <div>
        {#if element.type == "select"}
            <SelectForm {element} />
        {/if}
    </div>
    <div class="select_type">
        <label class="type_radio">
            <input type="radio" bind:group={element.type} value={"text"} />
            {dictionary.text}
        </label>
        <label class="type_radio">
            <input type="radio" bind:group={element.type} value={"number"} />
            {dictionary.number}
        </label>
        <label class="type_radio">
            <input type="radio" bind:group={element.type} value={"select"} />
            {dictionary.select}
        </label>
        <label class="type_radio">
            <input type="radio" bind:group={element.type} value={"table"} />
            {dictionary.table}
        </label>
        <label class="type_radio">
            <input type="radio" bind:group={element.type} value={"coordinates"} />
            {dictionary.coordinates}
        </label>
        <label class="type_radio">
            <input type="radio" bind:group={element.type} value={"photo"} />
            {dictionary.photo}
        </label>
        <!-- <button type="submit">Submit</button> -->
    </div>
    <hr />
    <!-- </form> -->
{/each}
<!-- {/if} -->
<button type="button" on:click={add_column}>add column</button>

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
    .select_type {
        display: flex;
    }
</style>
