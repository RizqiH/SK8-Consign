<script lang="ts">
	import { onMount } from 'svelte';

	let footerElement: HTMLElement;
	let isVisible = false;

	// Social media links
	const socialLinks = [
		{
			name: 'Instagram',
			icon: 'M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zm0-2.163c-3.259 0-3.667.014-4.947.072-4.358.2-6.78 2.618-6.98 6.98-.059 1.281-.073 1.689-.073 4.948 0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98 1.281.058 1.689.072 4.948.072 3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98-1.281-.059-1.69-.073-4.949-.073zm0 5.838c-3.403 0-6.162 2.759-6.162 6.162s2.759 6.163 6.162 6.163 6.162-2.759 6.162-6.163c0-3.403-2.759-6.162-6.162-6.162zm0 10.162c-2.209 0-4-1.79-4-4 0-2.209 1.791-4 4-4s4 1.791 4 4c0 2.21-1.791 4-4 4zm6.406-11.845c-.796 0-1.441.645-1.441 1.44s.645 1.44 1.441 1.44c.795 0 1.439-.645 1.439-1.44s-.644-1.44-1.439-1.44z',
			url: '#'
		},
		{
			name: 'Twitter',
			icon: 'M23.953 4.57a10 10 0 01-2.825.775 4.958 4.958 0 002.163-2.723c-.951.555-2.005.959-3.127 1.184a4.92 4.92 0 00-8.384 4.482C7.69 8.095 4.067 6.13 1.64 3.162a4.822 4.822 0 00-.666 2.475c0 1.71.87 3.213 2.188 4.096a4.904 4.904 0 01-2.228-.616v.06a4.923 4.923 0 003.946 4.827 4.996 4.996 0 01-2.212.085 4.936 4.936 0 004.604 3.417 9.867 9.867 0 01-6.102 2.105c-.39 0-.779-.023-1.17-.067a13.995 13.995 0 007.557 2.209c9.053 0 13.998-7.496 13.998-13.985 0-.21 0-.42-.015-.63A9.935 9.935 0 0024 4.59z',
			url: '#'
		},
		{
			name: 'TikTok',
			icon: 'M12.525.02c1.31-.02 2.61-.01 3.91-.02.08 1.53.63 3.09 1.75 4.17 1.12 1.11 2.7 1.62 4.24 1.79v4.03c-1.44-.05-2.89-.35-4.2-.97-.57-.26-1.1-.59-1.62-.93-.01 2.92.01 5.84-.02 8.75-.08 1.4-.54 2.79-1.35 3.94-1.31 1.92-3.58 3.17-5.91 3.21-1.43.08-2.86-.31-4.08-1.03-2.02-1.19-3.44-3.37-3.65-5.71-.02-.5-.03-1-.01-1.49.18-1.9 1.12-3.72 2.58-4.96 1.66-1.44 3.98-2.13 6.15-1.72.02 1.48-.04 2.96-.04 4.44-.99-.32-2.15-.23-3.02.37-.63.41-1.11 1.04-1.36 1.75-.21.51-.15 1.07-.14 1.61.24 1.64 1.82 3.02 3.5 2.87 1.12-.01 2.19-.66 2.77-1.61.19-.33.4-.67.41-1.06.1-1.79.06-3.57.07-5.36.01-4.03-.01-8.05.02-12.07z',
			url: '#'
		},
		{
			name: 'YouTube',
			icon: 'M23.498 6.186a3.016 3.016 0 0 0-2.122-2.136C19.505 3.545 12 3.545 12 3.545s-7.505 0-9.377.505A3.017 3.017 0 0 0 .502 6.186C0 8.07 0 12 0 12s0 3.93.502 5.814a3.016 3.016 0 0 0 2.122 2.136c1.871.505 9.376.505 9.376.505s7.505 0 9.377-.505a3.015 3.015 0 0 0 2.122-2.136C24 15.93 24 12 24 12s0-3.93-.502-5.814zM9.545 15.568V8.432L15.818 12l-6.273 3.568z',
			url: '#'
		}
	];

	// Quick links
	const quickLinks = [
		{ name: 'Tentang Kami', url: '#' },
		{ name: 'Cara Kerja', url: '#' },
		{ name: 'Syarat & Ketentuan', url: '#' },
		{ name: 'Kebijakan Privasi', url: '#' }
	];

	// Contact info
	const contactInfo = [
		{ label: 'Email', value: 'support@sk8consign.com' },
		{ label: 'Phone', value: '+62 812-3456-7890' },
		{ label: 'Address', value: 'Jl. Game Master No. 123, Jakarta' }
	];

	onMount(() => {
		const observer = new IntersectionObserver(
			(entries) => {
				entries.forEach((entry) => {
					if (entry.isIntersecting) {
						isVisible = true;
					}
				});
			},
			{ threshold: 0.1 }
		);

		if (footerElement) {
			observer.observe(footerElement);
		}

		return () => {
			if (footerElement) {
				observer.unobserve(footerElement);
			}
		};
	});
</script>

<footer bind:this={footerElement} class="relative overflow-hidden bg-black text-white">
	<!-- Neon grid background -->
	<div
		class="absolute inset-0 opacity-10"
		style="background-image: linear-gradient(to right, rgba(147, 51, 234, 0.2) 1px, transparent 1px),
             linear-gradient(to bottom, rgba(147, 51, 234, 0.2) 1px, transparent 1px);
             background-size: 40px 40px;"
	></div>

	<!-- Gradient overlay -->
	<div
		class="absolute inset-0"
		style="background: linear-gradient(135deg, rgba(147, 51, 234, 0.05) 0%, rgba(59, 130, 246, 0.05) 100%);"
	></div>

	<div class="relative mx-auto max-w-7xl px-4 py-16 sm:px-6 lg:px-8">
		<div class="grid grid-cols-1 gap-12 md:grid-cols-2 lg:grid-cols-4">
			<!-- Brand section -->
			<div
				class="space-y-6 transition-all duration-700 {isVisible
					? 'translate-y-0 opacity-100'
					: 'translate-y-10 opacity-0'}"
			>
				<h2 class="text-shadow-neon-purple text-3xl font-bold">SK8 Consign</h2>
				<p class="text-gray-400">
					Platform jual beli barang gaming terpercaya dengan pelayanan terbaik.
				</p>

				<!-- Social media icons -->
				<div class="flex space-x-4">
					{#each socialLinks as social, index}
						<a
							href={social.url}
							class="group transition-all duration-700 {isVisible
								? 'translate-y-0 opacity-100'
								: 'translate-y-5 opacity-0'}"
							style="transition-delay: {index * 100}ms"
							aria-label={social.name}
						>
							<div
								class="flex h-10 w-10 items-center justify-center rounded-lg
                           border border-purple-500/30 bg-black/50
                           backdrop-blur-sm transition-all duration-300 group-hover:border-purple-500/60
                           group-hover:shadow-[0_0_15px_rgba(147,51,234,0.3)]"
							>
								<svg
									class="h-5 w-5 fill-current text-purple-400 transition-colors group-hover:text-purple-300"
									viewBox="0 0 24 24"
								>
									<path d={social.icon} />
								</svg>
							</div>
						</a>
					{/each}
				</div>
			</div>

			<!-- Quick Links -->
			<div
				class="space-y-6 transition-all duration-700 {isVisible
					? 'translate-y-0 opacity-100'
					: 'translate-y-10 opacity-0'}"
				style="transition-delay: 200ms"
			>
				<h3 class="text-xl font-bold text-white">Quick Links</h3>
				<ul class="space-y-3">
					{#each quickLinks as link}
						<li>
							<a
								href={link.url}
								class="inline-block text-gray-400 transition-colors duration-300
                         hover:translate-x-2 hover:text-purple-400"
							>
								{link.name}
							</a>
						</li>
					{/each}
				</ul>
			</div>

			<!-- Contact Info -->
			<div
				class="space-y-6 transition-all duration-700 {isVisible
					? 'translate-y-0 opacity-100'
					: 'translate-y-10 opacity-0'}"
				style="transition-delay: 400ms"
			>
				<h3 class="text-xl font-bold text-white">Hubungi Kami</h3>
				<ul class="space-y-3">
					{#each contactInfo as info}
						<li class="flex items-start space-x-3">
							<span class="font-semibold text-purple-400">{info.label}:</span>
							<span class="text-gray-400">{info.value}</span>
						</li>
					{/each}
				</ul>
			</div>

			<!-- Newsletter -->
			<div
				class="space-y-6 transition-all duration-700 {isVisible
					? 'translate-y-0 opacity-100'
					: 'translate-y-10 opacity-0'}"
				style="transition-delay: 600ms"
			>
				<h3 class="text-xl font-bold text-white">Newsletter</h3>
				<p class="text-gray-400">Dapatkan update terbaru dari kami</p>
				<form class="space-y-3">
					<input
						type="email"
						placeholder="Email anda"
						class="w-full rounded-lg border border-purple-500/30 bg-black/50 px-4 py-2
                     text-white placeholder-gray-500 transition-colors duration-300
                     focus:border-purple-500/60 focus:outline-none"
					/>
					<button
						type="submit"
						class="w-full rounded-lg bg-purple-600 px-4 py-2 font-semibold
                     text-white transition-all duration-300 hover:bg-purple-700
                     hover:shadow-[0_0_15px_rgba(147,51,234,0.5)]"
					>
						Subscribe
					</button>
				</form>
			</div>
		</div>

		<!-- Bottom bar -->
		<div class="relative mt-16 border-t border-purple-500/20 pt-8">
			<div class="flex flex-col items-center justify-between space-y-4 md:flex-row md:space-y-0">
				<p class="text-sm text-gray-500">
					© {new Date().getFullYear()} SK8 Consign. All rights reserved.
				</p>
				<div class="flex space-x-6">
					<a href="#" class="text-sm text-gray-500 transition-colors hover:text-purple-400">Terms</a
					>
					<a href="#" class="text-sm text-gray-500 transition-colors hover:text-purple-400"
						>Privacy</a
					>
					<a href="#" class="text-sm text-gray-500 transition-colors hover:text-purple-400"
						>Cookies</a
					>
				</div>
			</div>
		</div>
	</div>

	<!-- Floating particles effect -->
	<div class="pointer-events-none absolute inset-0 overflow-hidden">
		{#each Array(15) as _, i}
			<div
				class="animate-float absolute h-1 w-1 rounded-full"
				style="
            left: {Math.random() * 100}%;
            top: {Math.random() * 100}%;
            animation-delay: {Math.random() * 5}s;
            animation-duration: {8 + Math.random() * 4}s;
            background-color: rgba(147, 51, 234, 0.3);
            box-shadow: 0 0 10px rgba(147, 51, 234, 0.3);
          "
			></div>
		{/each}
	</div>
</footer>

<style>
	.text-shadow-neon-purple {
		text-shadow:
			0 0 10px rgba(147, 51, 234, 0.7),
			0 0 20px rgba(147, 51, 234, 0.5),
			0 0 30px rgba(147, 51, 234, 0.3);
	}

	@keyframes float {
		0%,
		100% {
			transform: translateY(0) translateX(0);
		}
		25% {
			transform: translateY(-20px) translateX(10px);
		}
		50% {
			transform: translateY(-40px) translateX(-10px);
		}
		75% {
			transform: translateY(-20px) translateX(10px);
		}
	}

	.animate-float {
		animation: float 10s ease-in-out infinite;
	}
</style>
