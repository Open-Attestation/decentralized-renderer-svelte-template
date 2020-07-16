<script>
  export let document;
  export let rawDocument;
  export let handleObfuscation;

  let handleClick = () => {
    handleObfuscation("id");
  };
</script>

<style>
  .main {
    background-color: #676778;
    color: white;
    text-align: center;
    padding: 1em;
    margin: 0 auto;
  }
</style>

<div class="main">
  <h1>Hello from svelte</h1>
  <div>
    <button on:click={handleClick}>Hide field id</button>
  </div>
  <div>id: {document.id || ""}</div>
  <div>issued on: {document.issuedOn}</div>
  <div>description: {document.description}</div>
</div>
