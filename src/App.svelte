<script>
    import { Router, Route } from "svelte-routing";
    import NavLink from "./components/NavLink.svelte";
	import Home from "./routes/Home.svelte";
	import Blog from "./routes/Blog.svelte";
	import D3 from "./routes/D3.svelte";	
	export let url = "";
   
    import Tab, { Label } from '@smui/tab';
    import TabBar from '@smui/tab-bar';
    let active = 'Home';
</script>

   <Router url="{url}">
	   <div class="mdc-tab-bar" role="tablist">
		   <div class="mdc-tab-scroller__scroll-area mdc-tab-scroller__scroll-area--scroll" style="margin-bottom: 0px;">
			   <div class="mdc-tab-scroller__scroll-content" style="">
		            <button class="mdc-tab mdc-ripple-upgraded" role="tab"> <NavLink to="/">Home</NavLink></button>
					<button class="mdc-tab mdc-ripple-upgraded" role="tab"> <NavLink to="blog">Hope Data</NavLink></button>
					<button class="mdc-tab mdc-ripple-upgraded" role="tab"> <NavLink to="d3">D3</NavLink></button>
				</div>
			</div>
		</div>
   </Router>

    <Router url="{url}">
        <div>
		    <Route path="blog"><Blog/></Route>
		    <Route path="d3"><D3/></Route>
		    <Route path="/" ><Home/></Route>
	    </div>
    </Router>

