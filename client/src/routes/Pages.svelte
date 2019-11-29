<script context="module">
	import { gql } from "apollo-boost";
	import client from "../apollo.js";
	import { subscribe } from 'svelte-apollo';

	const PAGE_LIST = gql`
		query {
			page(order_by: [{id: asc}]) {
				id
				title
				slug
			}
		}
	`;

	export async function preload() {
		return {
			pagesCache: await client.query({ query: PAGE_LIST })
		};
	}

</script>

<script>
	import { restore, query } from 'svelte-apollo';

	export let pagesCache;
	restore(client, PAGE_LIST, pagesCache.data);

	const pages = query(client, { query: PAGE_LIST });

</script>

<ul>
	{#await $pages}
		<li>Loading...</li>
	{:then result}
		{#each result.data.page as page (page.id)}
			<li><a href="pages/{page.slug}">{page.title}</a></li>
		{:else}
			<li>No pages found</li>
		{/each}
	{:catch error}
		<li>Error loading pages: {error}</li>
	{/await}
</ul>