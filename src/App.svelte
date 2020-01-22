<script>
  import FramedDocumentRenderer from "./FramedDocumentRenderer.svelte";
  import Template from "./Template.svelte";

  let templates = [
    {
      id: "custom",
      label: "Custom",
      template: Template
    }
  ];
  let templateRegistry = {
    default: templates,
    custom: templates
  };
</script>

<style>
  body {
    margin: 0;
  }
  main {
    background-color: #676778;
    color: white;
    text-align: center;
    padding: 1em;
    margin: 0 auto;
  }
</style>

<main>
  <FramedDocumentRenderer {templateRegistry} />
</main>
