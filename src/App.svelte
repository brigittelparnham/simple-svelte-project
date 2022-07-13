<script>
    import { Router, Route } from "svelte-routing";
    import NavLink from "./components/NavLink.svelte";
	import Home from "./routes/Home.svelte";
	import Blog from "./routes/Blog.svelte";
	
	export let url = "";
</script>

<Router url="{url}">
	<nav>
		<NavLink to="/">Home</NavLink>
		<NavLink to="blog">Blog</NavLink>
	</nav>
	<div>
		<Route path="blog/*" component="{Blog}"/>
		<Route path="/" component="{Home}"/>
	</div>
</Router>

