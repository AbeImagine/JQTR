<style type="text/css">
	table, th, td {
	  border: 1px solid white;
	}
</style>

<script context="module">
	import { gql } from "apollo-boost";
	import client from "../apollo.js";
	import AddAlumno from './AddAlumno.svelte';

	const ALUMNO_LIST = gql`
		query {
			alumno(order_by: [{id: asc}]) {
				id
				nombre
				carrera
				generacion
			}
		}
	`;

	export async function preload() {
		return {
			alumnoCache: await client.query({ query: ALUMNO_LIST })
		};
	}

</script>

<script>
	import { restore, query } from 'svelte-apollo';

	export let alumnoCache;
	restore(client, ALUMNO_LIST, alumnoCache.data);

	const alumnos = query(client, { query: ALUMNO_LIST });
	const alumnosPreloading = preload();
</script>


{#await $alumnos}
	<p>Cargando datos...</p>
{:then result}
	<table>
		<tr>
			<th>Nombre</th>
			<th>Carrera</th>
			<th>Generación</th>
		</tr>
			{#each result.data.alumno as alumno (alumno.id)}
				<tr>
					<td>{alumno.nombre}</td>
					<td>{alumno.carrera}</td>
					<td>{alumno.generacion}</td>
				</tr>
			{:else}
				<p>No pages found</p>
			{/each}
	</table>

	{#await alumnosPreloading}
		<p>Preloading...</p>
	{:then preloaded}
		<AddAlumno {...preloaded}/>
	{/await}
{:catch error}
	<p>Error cargando la información: {error}</p>
{/await}