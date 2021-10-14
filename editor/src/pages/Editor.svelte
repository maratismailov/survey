<script>
  import Form from "../components/Form.svelte";
  import CurrentElement from "../components/CurrentElement.svelte";

  import {
    store_blank_element,
    store_survey,
    store_dictionary,
  } from "../stores.js";
  import { store_current_element, store_current_index} from "../stores.js";

  export let url;

  let is_show_survey = false;
  let is_show_element = false;
  let dictionary;

  const blank_element = {
    id: "new field",
    name: "",
    type: "",
    value: "",
    center: [],
    fields: [[]],
    select_values: [],
    selected: "",
    select: {
      table_name: "",
      name_column: "",
      where_clause: "",
      id_column: "",
    },
  };

  const blank_survey = {
    name: "",
    survey_id: "",
    objects_query_text: "",
    bounds_query_text: "",
    survey_body: [],
    initial_fields: [],
    object_code: "",
    geom_field: "",
    geom_type: "",
  };

  const blank_initial_field = {
    name: "",
  };

  let survey = { ...blank_survey };

  store_survey.set(survey);

  const unsubscribe = store_survey.subscribe((value) => {
    survey = value;
  });

  const unsubscribe2 = store_dictionary.subscribe((value) => {
    dictionary = value;
  });

  const add_field = () => {
    survey.survey_body.push({
      ...blank_element,
      select: { ...blank_element.select },
      fields: [...blank_element.fields],
    });
    store_survey.set(survey);
    store_current_element.set(survey.survey_body[survey.survey_body.length - 1])
    store_current_index.set(survey.survey_body.length - 1)
  };
  let surveys = [];
  // export let survey = {
  //   survey_id: "",
  //   name: "",
  //   survey_body: [],
  // };

  store_blank_element.set(blank_element);

  let element = { ...blank_element };

  const blank_column = {
    id: "",
    name_ru: "",
    name_en: "",
    name_kg: "",
    type: "",
    value: "",
  };
  let column = { ...blank_column };

  const save_survey_template = async () => {
    try {
      await fetch(url + `/save_survey_template?id=` + survey.survey_id, {
        method: "POST", // или 'PUT'
        body: JSON.stringify(survey),
        headers: {
          "Content-Type": "application/json",
        },
      });
    } catch (error) {
      console.error("Ошибка:", error);
    }
  };
  const save_survey = () => {
    surveys.push(survey);
    localStorage.setItem("surveys", JSON.stringify(surveys));
  };
</script>

<div>
  <div class="grid-container">
    <div>
      <Form {survey} {blank_initial_field} />
      <button on:click={add_field}>{dictionary.add_field}</button>
      {#if !is_show_element}
        <button on:click={() => (is_show_element = true)}>show element</button>
      {/if}
      {#if is_show_element}
        <button on:click={() => (is_show_element = false)}>hide element</button>
      {/if}
      {#if !is_show_survey}
        <button on:click={() => (is_show_survey = true)}>show survey</button>
      {/if}
      {#if is_show_survey}
        <button on:click={() => (is_show_survey = false)}>hide survey</button>
      {/if}
      <button on:click={save_survey}>save survey</button>
      <button on:click={save_survey_template}
        >{dictionary.save_survey_template}</button
      >
      <!-- <button on:click={delete_survey}>delete survey</button> -->

      {#if is_show_element}
        <pre>{JSON.stringify(element, undefined, 2)}</pre>
      {/if}
      {#if is_show_survey}
        <pre>{JSON.stringify(survey, undefined, 2)}</pre>
        <hr />
      {/if}
    </div>
    <div>
      <CurrentElement />
    </div>
  </div>
</div>

<style>
  .grid-container {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: 1fr;
    gap: 15px 15px;
  }
</style>
