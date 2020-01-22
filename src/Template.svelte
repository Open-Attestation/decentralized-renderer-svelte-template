<script>
  export let document;
  export let handleObfuscation;

  let handleClick = () => {
    handleObfuscation("id");
  };
</script>

<div>
  <h1>Hello from svelte</h1>
  <div>
    <button on:click={handleClick}>Hide field id</button>
  </div>
  <div>{JSON.stringify(document, null, 2)}</div>
</div>
