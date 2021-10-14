<script>
    import { store_current_element, store_current_index } from "../stores.js";

    export let element;
    export let index
    let current_index;
    let current_element

    const set_current_element = () => {
        store_current_element.set(element)
        store_current_index.set(index)
    };
    const unsubscribe = store_current_index.subscribe((value) => {
        current_index = value;
    });
    const unsubscribe2 = store_current_element.subscribe((value) => {
        current_element = value;
    });
</script>

<div on:click={set_current_element} class:selected="{index === current_index && current_element}">
    {element.id}
</div>

<style>
.selected {
    background-color: orange;
    color: #000;
}
</style>
