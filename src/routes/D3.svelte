<script>
    import ForceDirectedGraph from '../components/ForceDirectedGraph.svelte';
    import data from '../lib/d3-data.js'

</script>

<main>
<h2>D3 Force Directed Graph</h2>

<div class="chart">
	<svelte:component this={ForceDirectedGraph} graph={data}/>
</div>
</main>
