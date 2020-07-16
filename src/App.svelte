<script>
  import FramedDocumentRenderer from "./core/FramedDocumentRenderer.svelte";
  import Template from "./Template.svelte";

  let templates = [
    {
      id: "demo",
      label: "Demo",
      template: Template
    }
  ];
  let templateRegistry = {
    custom: templates
  };
</script>

<FramedDocumentRenderer {templateRegistry} />
