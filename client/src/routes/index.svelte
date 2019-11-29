<script context="module">
  // Here we import the graphql client
  import client from "../apollo.js";
  import { gql } from "apollo-boost";
  import Pages, { preload as pagePreload } from './Pages.svelte';
  import AddPage from './AddPage.svelte';
  import Alumnos, { preload as alumnosPreload } from './Alumnos.svelte';
  import SubPage from './SubPage.svelte';
  //import Chart from './Chart.svelte';

  const pagePreloading = pagePreload();
  const alumnosPreloading = alumnosPreload();
</script>

<h1>Pages</h1>

{#await pagePreloading}
  <p>Preloading page...</p>
{:then preloaded}
  <Pages {...preloaded} />
  <h2>Add Page</h2>
  <AddPage {...preloaded} />
{:catch error}
  <p>Error preloading pages: {error}</p>
{/await}