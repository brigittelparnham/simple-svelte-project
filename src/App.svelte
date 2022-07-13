<script>
    import { Router, Route } from "svelte-routing";
    import NavLink from "./components/NavLink.svelte";
	import Home from "./routes/Home.svelte";
	import Blog from "./routes/Blog.svelte";
	import D3 from "./routes/D3.svelte";	
	export let url = "";
</script>

<Router url="{url}">
	<nav>
		<NavLink to="/">Home</NavLink>
		<NavLink to="blog">Blog</NavLink>
		<NavLink to="d3">D3</NavLink>
	</nav>
	<div>
		<Route path="blog/*" component="{Blog}"/>
		<Route path="d3" component="{D3}"/>
		<Route path="/" component="{Home}"/>
	</div>
</Router>

