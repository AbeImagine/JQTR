<script context="module">
    import DBChart, { preload as dbChartPreload } from './DBChart.svelte';

    let data = [1,2,3,4,5,6];
    var myChart;
    var ctx;

    const dbChartPreloading = dbChartPreload();
</script>

<input type="number" bind:value={data[0]}>
<input type="number" bind:value={data[1]}>
<input type="number" bind:value={data[2]}>
<input type="number" bind:value={data[3]}>
<input type="number" bind:value={data[4]}>
<input type="number" bind:value={data[5]}>

<canvas id="myChart" width="2" height="1"></canvas>


{#await dbChartPreloading}
  <p>Preloading chart...</p>
{:then preloaded}
  <h2>Tabla de base de datos</h2>
  <DBChart {...preloaded} />
{:catch error}
  <p>Error preloading chart: {error}</p>
{/await}

<script>
    import {afterUpdate} from 'svelte';

    function createChart() {
        if(myChart) myChart.destroy();
        ctx = document.getElementById('myChart').getContext('2d');
        myChart = new Chart(ctx, {
            type: 'bar',
            data: {
                labels: ['Red', 'Blue', 'Yellow', 'Green', 'Purple', 'Orange'],
                datasets: [{
                    label: '# of Votes',
                    data,
                    backgroundColor: [
                        'rgba(255, 99, 132, 0.2)',
                        'rgba(54, 162, 235, 0.2)',
                        'rgba(255, 206, 86, 0.2)',
                        'rgba(75, 192, 192, 0.2)',
                        'rgba(153, 102, 255, 0.2)',
                        'rgba(255, 159, 64, 0.2)'
                    ],
                    borderColor: [
                        'rgba(255, 99, 132, 1)',
                        'rgba(54, 162, 235, 1)',
                        'rgba(255, 206, 86, 1)',
                        'rgba(75, 192, 192, 1)',
                        'rgba(153, 102, 255, 1)',
                        'rgba(255, 159, 64, 1)'
                    ],
                    borderWidth: 1
                }]
            },
            options: {
                scales: {
                    yAxes: [{
                        ticks: {
                            beginAtZero: true
                        }
                    }]
                }
            }
        });
    }


    afterUpdate(createChart);
</script>