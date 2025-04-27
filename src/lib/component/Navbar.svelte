<script lang="ts">
	import { onMount } from 'svelte';
	import { fade, fly, slide } from 'svelte/transition';
	import { quintOut } from 'svelte/easing';

	let isOpen = false;
	let scrolled = false;

	function toggleMenu() {
		isOpen = !isOpen;
	}

	function handleScroll() {
		scrolled = window.scrollY > 20;
	}

	onMount(() => {
		window.addEventListener('scroll', handleScroll);
		return () => window.removeEventListener('scroll', handleScroll);
	});

	const navLinks = [
		{ name: 'Home', href: '/' },
		{ name: 'Consign', href: '/consign' },
		{ name: 'Features', href: '/features' },
		{ name: 'About', href: '/about' }
	];
</script>

<nav
	class="fixed z-50 w-full transition-all duration-300
         {scrolled ? 'bg-black py-2 shadow-lg shadow-purple-500/10' : 'bg-transparent py-4'}"
>
	<div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
		<div class="flex items-center justify-between">
			<!-- Logo -->
			<a href="/" class="flex items-center space-x-2">
				<div class="relative">
					<span class="bg-white bg-clip-text text-2xl font-bold text-transparent"> SK8 </span>
					<div class="absolute -bottom-1 left-0 h-0.5 w-full bg-gray-400"></div>
				</div>
				<span class="text-sm text-gray-300">CONSIGN</span>
			</a>

			<!-- Desktop Navigation -->
			<div class="hidden items-center space-x-8 md:flex">
				{#each navLinks as link}
					<a
						href={link.href}
						class="group relative text-gray-300 transition-colors duration-200 hover:text-white"
					>
						{link.name}
						<span
							class="absolute -bottom-0 left-0 h-0.5 w-0 bg-gray-400 transition-all duration-300 group-hover:w-full"
						></span>
					</a>
				{/each}
			</div>

			<!-- Mobile menu button -->
			<button
				on:click={toggleMenu}
				class="relative p-2 text-gray-400 transition-colors hover:text-white md:hidden"
			>
				<div class="relative h-6 w-6">
					{#if isOpen}
						<svg
							class="absolute inset-0 h-6 w-6 rotate-0 transform"
							fill="none"
							stroke="currentColor"
							viewBox="0 0 24 24"
							in:fade={{ duration: 200 }}
							out:fade={{ duration: 200 }}
						>
							<path
								stroke-linecap="round"
								stroke-linejoin="round"
								stroke-width="2"
								d="M6 18L18 6M6 6l12 12"
							/>
						</svg>
					{:else}
						<svg
							class="absolute inset-0 h-6 w-6"
							fill="none"
							stroke="currentColor"
							viewBox="0 0 24 24"
							in:fade={{ duration: 200 }}
							out:fade={{ duration: 200 }}
						>
							<path
								stroke-linecap="round"
								stroke-linejoin="round"
								stroke-width="2"
								d="M4 6h16M4 12h16M4 18h16"
							/>
						</svg>
					{/if}
				</div>
			</button>
		</div>
	</div>

	<!-- Mobile menu -->
	{#if isOpen}
		<div
			class="fixed inset-0 top-[60px] bg-black/95 backdrop-blur-lg md:hidden"
			transition:fade={{ duration: 300 }}
		>
			<div class="space-y-3 px-4 pt-6 pb-6" transition:slide={{ duration: 400, easing: quintOut }}>
				{#each navLinks as link, index}
					<a
						href={link.href}
						on:click={toggleMenu}
						class="block rounded-lg border border-transparent px-4 py-3
                   text-lg text-gray-300 transition-all duration-300
                   hover:border-purple-500/30 hover:bg-purple-500/10 hover:text-white"
						in:fly={{ y: 20, duration: 300, delay: 100 * index }}
					>
						<div class="flex items-center justify-between">
							<span>{link.name}</span>
							<svg
								class="h-5 w-5 text-purple-400 opacity-0 transition-opacity group-hover:opacity-100"
								fill="none"
								stroke="currentColor"
								viewBox="0 0 24 24"
							>
								<path
									stroke-linecap="round"
									stroke-linejoin="round"
									stroke-width="2"
									d="M9 5l7 7-7 7"
								/>
							</svg>
						</div>
					</a>
				{/each}

				<!-- Mobile decorative elements -->
				<div class="mt-8 px-4 opacity-50">
					<div
						class="h-px bg-gradient-to-r from-transparent via-purple-500/50 to-transparent"
					></div>
				</div>

				<!-- Floating particles -->
				<div class="pointer-events-none absolute inset-0 overflow-hidden">
					{#each Array(10) as _, i}
						<div
							class="animate-float-mobile absolute h-1 w-1 rounded-full"
							style="
                left: {Math.random() * 100}%;
                top: {Math.random() * 100}%;
                animation-delay: {Math.random() * 5}s;
                animation-duration: {10 + Math.random() * 5}s;
                background: rgba(147, 51, 234, {0.3 + Math.random() * 0.3});
                box-shadow: 0 0 10px rgba(147, 51, 234, 0.3);
              "
						></div>
					{/each}
				</div>
			</div>
		</div>
	{/if}
</nav>

<style>
	@keyframes float-mobile {
		0%,
		100% {
			transform: translate(0, 0);
		}
		25% {
			transform: translate(10px, -10px);
		}
		50% {
			transform: translate(-5px, -20px);
		}
		75% {
			transform: translate(5px, -10px);
		}
	}

	.animate-float-mobile {
		animation: float-mobile 10s ease-in-out infinite;
	}
</style>
