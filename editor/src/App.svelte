<script>
  import { onMount } from "svelte";

  import Editor from "./pages/Editor.svelte";
  import TemplatesList from "./pages/TemplatesList.svelte";
  import { store_dictionary } from "./stores.js";
  let url = "http://192.168.20.35:8000";

  if (location.hostname == "localhost") {
    url = "http://192.168.20.35:8000";
  } else {
    url = "https://" + location.host + "/survey-editor";
  }

  if (!window.lng) {
    window.lng = "ru";
  }

  onMount(async () => {
    const res = await fetch(
      "https://forest.caiag.kg/" +
        window.lng +
        "/rent/taxdescr/getdictionarysurveyeditor"
    );
    const dictionary = await res.json();
    store_dictionary.set(dictionary);
  });
</script>

<div class="grid-container">
  <TemplatesList {url} />

  <Editor {url} />
</div>

<style>
  .grid-container {
    display: grid;
    grid-template-columns: 1fr 5fr;
    grid-template-rows: 1fr;
    gap: 15px 15px;
  }
</style>
