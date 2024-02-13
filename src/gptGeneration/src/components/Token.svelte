<script>
  export let buttonHandler;
  let apiKey;
</script>

<div class="content">
  <input type="text" placeholder="API KEY" bind:value={apiKey}>
  <button on:click={() => {buttonHandler(apiKey)}}>GO</button>
</div>

<style>
  input {
    width: 500px;
    height: 50px;
    margin-right: 20px;
    border: none;
    outline: none;
    box-sizing: border-box;
    transition: .1s linear;
    padding: 1% 2%;
    border-radius: 5px;
  }

  input:focus {
    border: 2px solid #646cff;
    outline: none;
  }

  button {
    width: 150px;
    height: 60px;
  }
</style>