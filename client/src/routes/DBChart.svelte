<script context="module">
	import { gql } from "apollo-boost";
    import client from "../apollo.js";
    import graphql2chartjs from 'graphql2chartjs';

    const VENTAS = gql`
        query {
            ventasDia(order_by: [{id: asc}]) {
                dia
                cantidad
            }
        }
    `;

    export async function preload() {
        return {
            ventasCache: await client.query({ query: VENTAS })
        };
    }
</script>

<script>
	import { restore, query } from 'svelte-apollo';
	import { afterUpdate } from 'svelte';
	export let ventasCache;
    restore(client, VENTAS, ventasCache.data);

    //const ventas = client.query(client, { query: VENTAS });
    const g2c = new graphql2chartjs();
    g2c.add(ventasCache.data, 'line');

    var ctx;
    var dbChart;

    function createChart(){
    	if(dbChart) dbChart.destroy();
		ctx = document.getElementById('dbChart').getContext('2d');
	    dbChart = new Chart(ctx, { type: 'line', data: g2c.data });
	}

	afterUpdate(createChart);
</script>

<canvas id="dbChart" width="2" height="1"></canvas>