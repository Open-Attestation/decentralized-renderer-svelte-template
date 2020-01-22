<script>
  import penpal from "penpal";
  export let templateRegistry;
  // abstract this =)
  let parent;
  try {
    const connection = penpal.connectToParent({
      methods: {
        renderDocument(doc, rawDoc) {
          document = doc;
          rawDocument = rawDoc;
          parent.updateHeight(1000);
        }
      }
    });
    connection.promise.then(p => {
      parent = p;
    });
  } catch (e) {
    console.error("Cant do this =)");
  }

  let document;
  let rawDocument;
  let Template;
  let templates;

  let getTemplates = document => {
    return templateRegistry[document.$template.name] || templateRegistry.default;
  };
  $: if (document) {
    templates = getTemplates(document);
    parent.updateTemplates(templates.map(({ id, label }) => ({ id, label })));
  }

  $: if (templates) {
    Template = getTemplates(document)[0].template;
  }
</script>

{#if parent && Template}
  <h1>
    <Template {document} {rawDocument} handleObfuscation={field => parent.handleObfuscation(field)} />
  </h1>
{/if}
{#if !parent}
  <h1>Loading...</h1>
{/if}
