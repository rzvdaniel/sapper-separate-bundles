<script context="module">
	export function preload({ params }) {
		return params
	}
</script>

<script>
	export let app

	let loading_component = new Promise(() => {})
	$: if (process.browser) {
		console.log(`loading app bundle '${app}.mjs'`)
		loading_component = import(`/bundles/${app}.mjs`)
	}
	
	// onMount is only called when this route is initialized
	// if you click a link from an app to another,
	// this won't be triggered
	import { onMount } from 'svelte'
	onMount(() => console.log(`[app].svelte route was mounted`))
</script>

<h1>App "{app}"</h1>

{#await loading_component}
	<strong>Loading...</strong>
{:then module}
	<svelte:component this="{module.default}"/>
{:catch err}
	<strong>Ops! Something went wrong: {err}</strong>
{/await}

<p><a href="/apps">back to list</a></p>
