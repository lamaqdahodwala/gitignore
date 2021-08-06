<script>
	import { camelCase } from "../node_modules/lodash";
	let name = "";
	let gitignore;

	async function get_gitignore_data() {
		let resp = await fetch(
			`https://gitlab.com/api/v4/templates/gitignores/${
				name.charAt(0).toUpperCase() + name.substring(1)
			}`
		);
		let json = await resp.json();
		return json.content.replaceAll("\n", "<br>");
	}
</script>

<svelte:head>
	<title>Gitignore chooser</title>
</svelte:head>
<br /><br />
<div class="container">
	<input
		class="input"
		type="text"
		bind:value={name}
		on:change={() => (gitignore = get_gitignore_data())}
	/>

	<hr />
	<h2>Gitignore:</h2>
	{#if gitignore}
		{#await gitignore}
			<progress class="progress" />
		{:then data}
			<pre>
				{@html data}
			</pre>
		{:catch}
			<p>No gitignore matches your query. Try using different words</p>
		{/await}
	{:else}
		<p></p>
	{/if}
</div>

<style>
	@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300&display=swap');

	:root {
		font-family: 'Roboto', sans-serif;
	}
	pre {
		background-color: rgb(173, 173, 173);
		border-radius: 10px;
		padding: 1rem;
	}

	.container {
		margin-left: 1rem;
		margin-right: 1rem;
	}

	.input {
		width: 100%;
		height: 2.5em;
		border-radius: 10px;
	}

	hr {
		margin-top: 1em;
		margin-bottom: 1em;
	}	
</style>
