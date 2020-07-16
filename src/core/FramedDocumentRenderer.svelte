<script>
  import penpal from "penpal";
  import mutation from "svelte-actions-mutation";
  import DefaultTemplate from "./DefaultTemplate.svelte";

  export let templateRegistry;

  const defaultTemplate = [
    {
      id: "default-template",
      label: "Default",
      template: DefaultTemplate
    }
  ];

  const resize = () => {
    // using https://stackoverflow.com/questions/1145850/how-to-get-height-of-entire-document-with-javascript
    const body = window.document.body,
      html = window.document.documentElement;

    const height = Math.max(
      body.scrollHeight,
      body.offsetHeight,
      html.clientHeight,
      html.scrollHeight,
      html.offsetHeight
    );
    parent.dispatch({ type: "UPDATE_HEIGHT", payload: height });
  }

  const mutationOptions = {
    attributeFilter: ["class"],
    subtree: true,
    attributes() {
      resize();
    },
    childList() {
      resize();
    },
    characterData() {
      resize();
    }
  };

  let parent;
  let selectedTemplate;
  try {
    const connection = penpal.connectToParent({
      methods: {
        dispatch: ({ type, payload }) => {
          if (type === "RENDER_DOCUMENT") {
            document = payload.document;
            rawDocument = payload.rawDocument;
          } else if (type === "PRINT") {
            window.print();
          } else if (type === "SELECT_TEMPLATE") {
            selectedTemplate = payload;
          } else {
            console.error(`Unable to handle ${type}`);
          }
        }
      }
    });
    connection.promise.then(p => {
      parent = p;
    });
  } catch (e) {
    console.error("Error:", e);
  }
  const handleObfuscation = field => {
    parent.dispatch({ type: "OBFUSCATE", payload: field });
  };

  let document;
  let rawDocument;
  let Template;
  let templates;

  let getTemplates = document => {
    return templateRegistry[document.$template.name] || defaultTemplate;
  };
  $: if (document) {
    templates = getTemplates(document);
    selectedTemplate = templates[0].id;
    parent.dispatch({ type: "UPDATE_TEMPLATES", payload: templates.map(({ id, label }) => ({ id, label })) });
  }

  $: if (templates && selectedTemplate) {
    Template = getTemplates(document).find(template => template.id === selectedTemplate).template;
    setTimeout(resize);
  }
</script>

<svelte:window on:resize={resize} />
{#if parent && Template}
  <div use:mutation={mutationOptions}>
    <svelte:component this={Template} {document} {rawDocument} handleObfuscation={field => handleObfuscation(field)} />
  </div>
{/if}
{#if !parent || !Template}
  <h1>Loading...</h1>
{/if}
