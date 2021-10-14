<script>
    import SelectForm from "../components/SelectForm.svelte";
    import TableForm from "../components/TableForm.svelte";

    import {
        store_current_element,
        store_blank_element,
        store_survey,
        store_current_index,
        store_dictionary,
    } from "../stores.js";

    let element;
    let blank_element;
    let global_index;
    let survey;
    let dictionary

    const unsubscribe = store_current_element.subscribe((value) => {
        element = value;
    });
    const unsubscribe2 = store_blank_element.subscribe((value) => {
        blank_element = value;
    });

    const unsubscribe3 = store_survey.subscribe((value) => {
        survey = value;
    });

    const unsubscribe5 = store_current_index.subscribe((value) => {
        global_index = value;
    });

    const unsubscribe6 = store_dictionary.subscribe((value) => {
        dictionary = value;
    });

    const move = (array, index, delta) => {
        //ref: https://gist.github.com/albertein/4496103
        //   console.log('move', array, index, delta);
        let newIndex = index + delta;
        if (newIndex < 0 || newIndex == array.length) return; //Already at the top or bottom.
        let indexes = [index, newIndex].sort((a, b) => a - b); //Sort the indixes (fixed)
        array.splice(indexes[0], 2, array[indexes[1]], array[indexes[0]]); //Replace from lowest index, two elements, reverting the order
        array = array;
        store_survey.set(survey);
        store_current_index.set(newIndex);
        global_index = newIndex;
        element = survey.survey_body[newIndex];
        store_current_element.set(element);
        // element = element
    };

    // const submit_element = () => {
    //     console.log("blank", blank_element);
    //     console.log(element);
    //     survey.survey_body.push(element);
    //     survey = survey;
    //     element = { ...blank_element };
    //     element.fields = [];
    //     // column = { ...blank_column };
    // };
</script>

{#if element}
    <!-- <form on:submit|preventDefault=> -->
    <div class="form_element">
        <div class="form_left">{dictionary.element_id}</div>
        <div class="form_right">
            <input bind:value={element.id} on:input={() => store_survey.set(survey)} type="text" />
        </div>
    </div>

    <div class="form_element">
        <div class="form_left">{dictionary.element_name}</div>
        <div class="form_right">
            <input bind:value={element.name} type="text" />
        </div>
    </div>

    <div>
        {#if element.type == "table"}
            <TableForm {element} {blank_element} />
        {/if}
        {#if element.type == "select"}
            <SelectForm {element} {blank_element} />
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
    <button
        on:click={() => move(survey.survey_body, global_index, -1)}
        disabled={global_index <= 0}>{dictionary.move_up}</button
    >
    <button
        on:click={() => move(survey.survey_body, global_index, 1)}
        disabled={global_index >= survey.survey_body.length - 1}
        >{dictionary.move_down}</button
    >
    <!-- </form> -->
{/if}

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
    .type_radio {
        margin: 5px;
    }
</style>
