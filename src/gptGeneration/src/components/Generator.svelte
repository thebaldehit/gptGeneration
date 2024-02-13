<script>
	export let token;

	let selectedOption;
	let isTextGeneration;

	let systemPrompt;
	let userPrompt;
	let imagePrompt;

	let output = '';
	
	const getModelsList = async () => {
		const request = await fetch('https://api.openai.com/v1/models', {
			method: 'GET',
			headers: {
				'Authorization': `Bearer ${token}`
			}
		});
		const response = await request.json();
		return response.data.filter(item => item.id.startsWith('gpt') || item.id.startsWith('dall'));
	};

	const handleSelection = () => {
		isTextGeneration = selectedOption.startsWith('gpt');
	};

	const generateText = async () => {
		output = 'Generating...';
		const request = await fetch('https://api.openai.com/v1/chat/completions', {
			method: 'POST', 
			headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${token}`
      },
			body: JSON.stringify({
        model: selectedOption,
        messages: [
          {
            role: 'system',
            content: systemPrompt
          },
					{
						role: 'user',
						content: userPrompt
					}
        ]
      })
		});
		const response = await request.json();
		if (!request.ok) output = 'Something went wrong' + response.error.message;
		output = response.choices[0].message.content;
	};

	const generateImage = async () => {
		output = 'Generating...';
		const request = await fetch('https://api.openai.com/v1/images/generations', {
			method: 'POST', 
			headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${token}`
      },
			body: JSON.stringify({
        model: selectedOption,
        prompt: imagePrompt,
				n: 1,
				size: '1024x1024'
      })
		});
		const response = await request.json();
		console.log(response);
		if (!request.ok) output = 'Something went wrong' + response.error.message;
		output = response.data[0].url;
	};
</script>

<div class="content">
	<p>Model:
		<select bind:value={selectedOption} on:change={handleSelection}>
			{#await getModelsList()}
			{:then models} 
				<option value="">Choose an option</option>
				{#each models as option (option.id)}
					<option value={option.id}>{option.id}</option>
				{/each}
			{/await}
		</select>
	</p>

	{#if isTextGeneration && selectedOption}
		<input type="text" placeholder="System prompt" bind:value={systemPrompt}>
		<br>
		<input type="text" placeholder="User prompt" bind:value={userPrompt}>
		<br>
		<button on:click={generateText}>Generate</button>
	{:else if selectedOption}
		<input type="text" placeholder="Image prompt" bind:value={imagePrompt}>
		<br>
		<button on:click={generateImage}>Generate</button>
	{/if}

	<div class="output">
		{output}
	</div>
</div>

<style>
	.content {
		font-size: 20px;
	}

	select {
		height: 40px;
		width: 300px;
		margin-left: 10px;
	}

	input {
		font-size: 14px;
    width: 400px;
    height: 50px;
    border: none;
		margin-bottom: 20px;
    outline: none;
    box-sizing: border-box;
    transition: .1s linear;
    padding: 1% 3%;
    border-radius: 5px;
  }

  input:focus {
    border: 2px solid #646cff;
    outline: none;
  }

	button {
    width: 400px;
	}

	.output {
		margin-top: 50px;
		max-width: 1000px;
	}
</style>