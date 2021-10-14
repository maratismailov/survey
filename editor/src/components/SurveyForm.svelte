<script>
  import { store_dictionary } from "../stores.js";

  export let survey;
  export let blank_initial_field;

  let dictionary;

  const unsubscribe = store_dictionary.subscribe((value) => {
    dictionary = value;
  });

  const add_initial_field = () => {
    survey.initial_fields = [
      ...survey.initial_fields,
      { ...blank_initial_field },
    ];
  };
  const delete_initial_field = (array, index) => {
    array.splice(index, 1);
    survey.initial_fields = survey.initial_fields;
  };

  // const add_field = () => {
  //   survey.survey_body.push({...blank_element})
  //   store_survey.set(survey)
  // }
  //     const unsubscribe = survey.subscribe((value) => {
  //     survey = value;
  //   });
</script>

<div class="form_element">
  <div class="form_left">{dictionary.survey_id}</div>
  <div class="form_right">
    <input bind:value={survey.survey_id} type="text" />
  </div>
</div>

<div class="form_element">
  <div class="form_left">{dictionary.survey_name}</div>
  <div class="form_right">
    <input bind:value={survey.name} type="text" />
  </div>
</div>
<div class="form_element">
  <div class="form_left">{dictionary.object_code}</div>
  <div class="form_right">
    <input bind:value={survey.object_code} type="text" />
  </div>
</div>
<div class="form_element">
  <div class="form_left">{dictionary.geom_field}</div>
  <div class="form_right">
    <input bind:value={survey.geom_field} type="text" />
  </div>
</div>
<div class="form_element">
  <div class="form_left">{dictionary.geom_type}</div>
  <div class="form_right">
    <label class="type_radio">
      <input type="radio" bind:group={survey.geom_type} value={"polygon"} />
      {dictionary.geom_type_polygon}
    </label>
    <label class="type_radio">
      <input type="radio" bind:group={survey.geom_type} value={"point"} />
      {dictionary.geom_type_point}
    </label>
  </div>
</div>
<hr />
<button on:click={add_initial_field}>{dictionary.add_initial_field}</button>
{#each survey.initial_fields as initial_field, index}
  <!-- <div class="form_element">
    <div class="form_left">{dictionary.element_id}</div>
    <div class="form_right">
      <input bind:value={initial_field.id} type="text" />
    </div>
  </div> -->

  <div class="form_element">
    <div class="form_left">{dictionary.element_name}</div>
    <div class="form_right">
      <input style="width:88%" bind:value={initial_field.name} type="text" />
      <input
        type="image"
        img
        src="assets/icons/trash.svg"
        class="delete"
        alt="delete"
        on:click={delete_initial_field(survey.initial_fields, index)}
      />
    </div>
  </div>
{/each}
<div class="form_element">
  <div class="form_left">{dictionary.objects_query_text}</div>
  <div class="form_right">
    <textarea bind:value={survey.objects_query_text} type="text" />
  </div>
</div>
<div class="form_element">
  <div class="form_left">{dictionary.bounds_query_text}</div>
  <div class="form_right">
    <textarea bind:value={survey.bounds_query_text} type="text" />
  </div>
</div>

<style>
  .form_element {
    display: flex;
    margin: auto auto;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .form_left {
    width: 230px;
  }
  .form_right {
    display: flex;
    flex-grow: 1;
    width: 300px;
  }
  textarea {
    height: 200px;
    width: 100%;
  }
  .delete {
    background-color: orange;
    size: 12px;
    vertical-align: bottom;
  }
  .select_type {
    display: flex;
  }
  .type_radio {
    margin: 5px;
  }
</style>
