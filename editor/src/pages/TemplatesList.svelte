<script>
    import { onMount } from "svelte";

    import { store_current_element, store_survey } from "../stores.js";

    export let url;
    let templates_list;

    onMount(async () => {
        let server = url + `/get_templates_list`
        const res = await fetch(server);
        const result = await res.json();
        templates_list = JSON.parse(result);
    });
    const get_survey_by_id = async (id) => {
        let server = url + `/get_template_by_id?id=` + id
        const res = await fetch(server);
        const result = await res.json();
        store_survey.set(JSON.parse(result).survey);
        store_current_element.set(null);
    };

</script>

<div>
    {#if templates_list}
        {#each templates_list as template}
            <div on:click={get_survey_by_id(template.survey_id)}>
                <div>
                    {template.survey_id}
                </div>
                <div>
                    {template.survey_name}
                </div>
                <hr />
            </div>
        {/each}
    {/if}
</div>
