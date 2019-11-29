<style type="text/css">
	#alumno-carrera {
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

	const ADD_ALUMNO = gql`
		mutation AddAlumno($nombre: String!, $carrera: String!, $generacion: Int!) {
			insert_alumno(objects: {nombre: $nombre, carrera: $carrera, generacion: $generacion}){
				affected_rows
			}
		}
	`;

	let nombre = '';
	let carrera = '';
	let generacion = '';
	export let alumnoCache;

	async function addAlumno(e) {
		e.preventDefault();
		try {
			await mutate(client, {
				mutation: ADD_ALUMNO,
				variables: { nombre, carrera, generacion }
			});
			alert("Page created successfully!");
			const finalData = alumnoCache.data.alumno;
			finalData.push({nombre, carrera, generacion, '__typename': 'alumno'});
			restore(client, ALUMNO_LIST, {alumno: finalData});

			nombre = '';
			carrera = '';
			generacion = '';
		}
		catch(error){
			console.error(error);
		}
	}
</script>

<form on:submit={addAlumno}>
	<label for="page-title">Nombre</label>
	<input type="text" id="alumno-nombre" bind:value={nombre} />

	<div class="body">
		<label for="page-body">Carrera</label>
		<input type="text" id="alumno-carrera" bind:value={carrera} />
	</div>
	
	<label for="slug">Slug</label>
	<input type="number" id="alumno-generacion" bind:value={generacion}>
	<button type="submit" id="boton">Agregar Alumno</button>
</form>