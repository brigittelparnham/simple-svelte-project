<!-- Copyright 2021 Observable, Inc.
Released under the ISC license.
https://observablehq.com/@d3/force-directed-graph-->
<script>
	import { onMount } from 'svelte';

	import { scaleLinear, scaleOrdinal } from 'd3-scale';
    import { zoom, zoomIdentity } from 'd3-zoom';
	import { schemeCategory10 } from 'd3-scale-chromatic';
	import { select, selectAll } from 'd3-selection';
	import { drag } from 'd3-drag';
	import { forceSimulation, forceLink, forceManyBody, forceCenter } from 'd3-force';

	let d3 = { zoom, zoomIdentity, scaleLinear, scaleOrdinal, schemeCategory10, select, selectAll, drag,  forceSimulation, forceLink, forceManyBody, forceCenter }

	export let graph;

	let svg;
	let width = 700;
	let height = 600;
    const nodeRadius = 5;

	const padding = { top: 20, right: 40, bottom: 40, left: 25 };

	$: links = graph.links.map(d => Object.create(d));
	$: nodes = graph.nodes.map(d => Object.create(d));  

	const colourScale = d3.scaleOrdinal(d3.schemeCategory10);

	let transform = d3.zoomIdentity;
    let simulation
	onMount(() => {

	simulation = d3.forceSimulation(nodes)
		.force("link", d3.forceLink(links).id(d => d.id))
		.force("charge", d3.forceManyBody())
		.force("center", d3.forceCenter(width / 2, height / 2))
		.on('tick', simulationUpdate);

	d3.select(svg)
		.call(d3.drag()
			.container(svg)
			.subject(dragsubject)
			.on("start", dragstarted)
			.on("drag", dragged)
			.on("end", dragended))
		.call(d3.zoom()
          .scaleExtent([1 / 10, 8])
          .on('zoom', zoomed));
	});

	function simulationUpdate () {
		simulation.tick();
		nodes = [...nodes];
		links = [...links];
	}

    function zoomed(currentEvent) {
        transform = currentEvent.transform;
        simulationUpdate();
    }

	function dragsubject(currentEvent) {
        const node = simulation.find(transform.invertX(currentEvent.x), transform.invertY(currentEvent.y), nodeRadius);
        if (node) {
            node.x = transform.applyX(node.x);
            node.y = transform.applyY(node.y);
        }
        return node;
	}

    function dragstarted(currentEvent) {
        if (!currentEvent.active) simulation.alphaTarget(0.3).restart();
        currentEvent.subject.fx = transform.invertX(currentEvent.subject.x);
        currentEvent.subject.fy = transform.invertY(currentEvent.subject.y);
    }

    function dragged(currentEvent) {
        currentEvent.subject.fx = transform.invertX(currentEvent.x);
        currentEvent.subject.fy = transform.invertY(currentEvent.y);
    }

    function dragended(currentEvent) {
        if (!currentEvent.active) simulation.alphaTarget(0);
        currentEvent.subject.fx = null;
        currentEvent.subject.fy = null;
    }

	function resize() {
		({ width, height } = svg.getBoundingClientRect());
	}


</script>

<svelte:window on:resize='{resize}'/>

<!-- SVG was here -->
<svg bind:this={svg} width='{width}' height='{height}'>
	{#each links as link}
    <g stroke='#999' stroke-opacity='0.6'>
      <line x1='{link.source.x}' y1='{link.source.y}' 
            x2='{link.target.x}' y2='{link.target.y}'
            transform='translate({transform.x} {transform.y}) scale({transform.k} {transform.k})'>
            <title>{link.source.id}</title>
      </line>
    </g>
	{/each}

	{#each nodes as point}
    <circle class='node' r='5' fill='{colourScale(point.group)}' cx='{point.x}' cy='{point.y}'
     transform='translate({transform.x} {transform.y}) scale({transform.k} {transform.k})'>
    <title>{point.id}</title></circle>
	{/each}

</svg>
