<script lang="ts">
	import '../app.css';
	import Navbar from '$lib/component/Navbar.svelte';
	import Scroll from '$lib/component/Scroll.svelte';
	import Footer from '$lib/component/Footer.svelte';
	let { children } = $props();
</script>

<Navbar />
<main class="flex-grow relative overflow-hidden">
	{@render children()}
</main>
<Scroll  />
<Footer />
