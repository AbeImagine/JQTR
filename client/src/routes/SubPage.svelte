<script context="module">
	import { gql } from "apollo-boost";
	import client from "../apollo.js";
	import { subscribe } from 'svelte-apollo';

	const PAGE_SUB = gql`
		subscription {
			page(order_by: [{title: asc}]) {
				title
				slug
			}
		}
	`;

	const pageList = subscribe(client, { query: PAGE_SUB });

</script>

<h2>Subscription test</h2>

<ul>
	{#await $pageList}
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