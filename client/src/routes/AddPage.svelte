<style type="text/css">
	#page-body {
		display: block;
	}

	.body {
		margin: 10px 0px;
	}
	#boton{
		background-color: #17819B;
	}
</style>

<script>
	import client from '../apollo.js';
	import { gql } from 'apollo-boost';
	import { restore, mutate } from 'svelte-apollo';

	const PAGE_LIST = gql`
		query {
			page(order_by: [{id: asc}]) {
				id
				title
				slug
			}
		}
	`;

	const ADD_PAGE = gql`
		mutation AddPage($title: String!, $body: String!, $slug: String!) {
			insert_page(objects: {title: $title, body: $body, slug: $slug}){
				affected_rows
			}
		}
	`;

	let title = '';
	let body = '';
	let slug = '';
	export let pagesCache;

	async function addPage(e) {
		e.preventDefault();
		try {
			await mutate(client, {
				mutation: ADD_PAGE,
				variables: { title, body, slug }
			});
			alert("Page created successfully!");
			const finalData = pagesCache.data.page;
			finalData.push({title, '__typename': 'page'});
			restore(client, PAGE_LIST, {page: finalData});

			title = '';
			body = '';
			slug = '';
		}
		catch(error){
			console.error(error);
		}
	}
</script>

<form on:submit={addPage}>
	<label for="page-title">Title</label>
	<input type="text" id="page-title" bind:value={title} />

	<div class="body">
		<label for="page-body">Body</label>
		<textarea rows="6" cols="50" bind:value={body} id="page-body"></textarea>	
	</div>
	
	<label for="slug">Slug</label>
	<input type="text" id="page-slug" bind:value={slug}>
	<button type="submit" id="boton">Add Page</button>
</form>