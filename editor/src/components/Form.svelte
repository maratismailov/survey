<script>
  import SurveyForm from "./SurveyForm.svelte";
  import ElementForm from "./ElementForm.svelte";

  export let survey;
  // export let element;
  // export let column;
  // export let blank_column;
  export let blank_initial_field;

  // const add_column = () => {
  //   element.fields.push(column);
  //   column = { ...blank_column };
  //   document.getElementById("select_type").selectedIndex = 0;
  // };
  const delete_element = (index) => {
    survey.survey_body = survey.survey_body.filter((element, i) => i !== index);
  };
</script>

<SurveyForm {survey} {blank_initial_field} />

<hr />

{#if survey}
  <div class="elements">
    {#each survey.survey_body as element, index}
      <div class="form_right">
        <ElementForm {element} {index} />
        <input
          type="image"
          img
          src="assets/icons/trash.svg"
          class="delete"
          alt="delete"
          on:click={() => delete_element(index)}
        />
      </div>
      <hr />
    {/each}
  </div>
{/if}

<style>
  .form_right {
    display: flex;
    flex-grow: 1;
    justify-content: space-between;
  }
  .delete {
    background-color: orange;
    size: 5px;
    font-family: inherit;
    font-size: 6;
    -webkit-padding: 0.4em 0;
    padding: 0.1em;
    margin: 0 0 0.5em 0;
  }
  .elements {
    overflow: auto;
    max-height: 350px;
  }
</style>
