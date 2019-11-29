<script context="module">
	import client from "../../apollo.js";
	import { gql } from "apollo-boost";

	export let slug;

	const PAGE_GET = gql`
		query pageGet($slug: String!) {
		  	page(where: {slug: {_eq: $slug}}) {
		    	slug
		    	title
			    id
			    body
		  	}
		}
	`;

	export async function preload({ params, query }) {
		const slug = params.slug;
		return {
			pageCache: await client.query({ query: PAGE_GET, variables: {slug} })
		};
	}
</script>

<script>
	import { restore, query } from 'svelte-apollo';

	export let pageCache;
	restore(client, PAGE_GET, pageCache.data);

	const pages = query(client, { query: PAGE_GET, variables: {slug} });
</script>


{#await $pages}
	<h3>Loading...</h3>
{:then result}
	{#each result.data.page as page (page.id)}
		<h1>{page.title}</h1>

		<p>{@html page.body}</p>
	{:else}
		<h1>No pages found</h1>
	{/each}
{:catch error}
	<h3>Error loading: {error}</h3>
{/await}