<script>
  import Token from "./components/Token.svelte";

  let token = localStorage.getItem('OpenAIToken');

  const verifyToken = async (apiKey) => {
    const request = await fetch('https://api.openai.com/v1/chat/completions', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${apiKey}`
      },
      body: JSON.stringify({
        model: 'gpt-3.5-turbo',
        messages: [
          {
            role: 'system',
            content: 'it is a test'
          }
        ]
      })
    });
    if (request.ok) {
      localStorage.setItem('OpenAIToken', apiKey);
      token = apiKey;
    }
  };
</script>

<div class="container">
  {#if token}
  {:else}
    <Token buttonHandler={verifyToken}/>
  {/if}
</div>