<script lang="ts">
	import { onMount } from 'svelte';

	let heroVisible = false;
	let mainFeaturesVisible = false;
	let statsVisible = false;

	const mainFeatures = [
		{
			title: 'Smart Authentication',
			description:
				'Sistem otentikasi barang yang canggih untuk memastikan keaslian produk gaming yang Anda beli atau jual.',
			icon: 'M9 12l2 2 4-4m5.618-4.016A11.955 11.955 0 0112 2.944a11.955 11.955 0 01-8.618 3.04A12.02 12.02 0 003 9c0 5.591 3.824 10.29 9 11.622 5.176-1.332 9-6.03 9-11.622 0-1.042-.133-2.052-.382-3.016z',
			gradient: 'from-purple-500 to-blue-500'
		},
		{
			title: 'Express Shipping',
			description:
				'Layanan pengiriman cepat dengan tracking real-time untuk memastikan barang Anda sampai dengan aman.',
			icon: 'M13 10V3L4 14h7v7l9-11h-7z',
			gradient: 'from-blue-500 to-cyan-500'
		},
		{
			title: 'Secure Payment',
			description:
				'Sistem pembayaran yang aman dengan berbagai metode pembayaran untuk kenyamanan transaksi Anda.',
			icon: 'M3 10h18M7 15h1m4 0h1m-7 4h12a3 3 0 003-3V8a3 3 0 00-3-3H6a3 3 0 00-3 3v8a3 3 0 003 3z',
			gradient: 'from-cyan-500 to-purple-500'
		},
		{
			title: '24/7 Support',
			description:
				'Tim support yang siap membantu Anda kapan saja untuk menyelesaikan semua kendala transaksi.',
			icon: 'M18.364 5.636l-3.536 3.536m0 5.656l3.536 3.536M9.172 9.172L5.636 5.636m3.536 9.192l-3.536 3.536M21 12a9 9 0 11-18 0 9 9 0 0118 0zm-5 0a4 4 0 11-8 0 4 4 0 018 0z',
			gradient: 'from-purple-500 to-pink-500'
		}
	];

	const stats = [
		{ number: '1M+', label: 'Active Users' },
		{ number: '500K+', label: 'Successful Transactions' },
		{ number: '99.9%', label: 'Customer Satisfaction' },
		{ number: '24/7', label: 'Support Available' }
	];

	onMount(() => {
		const observerOptions = { threshold: 0.1 };

		const observers = [
			{ element: 'hero-section', callback: () => (heroVisible = true) },
			{ element: 'main-features', callback: () => (mainFeaturesVisible = true) },
			{ element: 'stats-section', callback: () => (statsVisible = true) }
		];

		const intersectionObservers = observers.map(({ element, callback }) => {
			const observer = new IntersectionObserver((entries) => {
				entries.forEach((entry) => {
					if (entry.isIntersecting) callback();
				});
			}, observerOptions);

			const el = document.getElementById(element);
			if (el) observer.observe(el);

			return observer;
		});

		return () => {
			intersectionObservers.forEach((observer) => observer.disconnect());
		};
	});
</script>

<div class="bg-black text-white">
	<!-- Hero Section -->
	<section
		id="hero-section"
		class="relative flex min-h-[75vh] items-center justify-center overflow-hidden"
	>
		<!-- Animated background -->
		<div class="absolute inset-0">
			<div class="absolute inset-0 bg-gradient-to-br from-black via-purple-900/30 to-black"></div>
			<div
				class="absolute inset-0 opacity-30"
				style="background-image: radial-gradient(circle at center, rgba(147, 51, 234, 0.1) 0%, transparent 70%);
                 background-size: 100px 100px;
                 animation: pulse 4s ease-in-out infinite;"
			></div>
		</div>

		<!-- Content -->
		<div
			class="relative z-10 px-4 text-center transition-all duration-1000
                  {heroVisible ? 'translate-y-0 opacity-100' : 'translate-y-10 opacity-0'}"
		>
			<h1
				class="animate-gradient mb-6 bg-gradient-to-r from-purple-400 via-pink-500 to-cyan-400 bg-clip-text text-6xl
                   font-bold text-transparent md:text-7xl"
			>
				Powerful Features
			</h1>
			<p class="mx-auto mb-8 max-w-3xl text-xl text-gray-300">
				Temukan fitur-fitur canggih yang membuat SK8 Consign menjadi platform jual beli gaming
				terdepan di Indonesia
			</p>
			<div class="flex justify-center gap-4">
				<button
					class="transform rounded-full bg-gradient-to-r from-purple-600 to-blue-600 px-8 py-3
                       font-semibold transition-all duration-300 hover:scale-105 hover:from-purple-700 hover:to-blue-700"
				>
					Get Started
				</button>
				<button
					class="rounded-full border border-purple-500/50 px-8 py-3 font-semibold
                       transition-all duration-300 hover:bg-purple-500/10"
				>
					Learn More
				</button>
			</div>
		</div>

		<!-- Floating elements -->
		<div class="pointer-events-none absolute inset-0 overflow-hidden">
			{#each Array(20) as _, i}
				<div
					class="animate-float-random absolute h-2 w-2 rounded-full"
					style="
              left: {Math.random() * 100}%;
              top: {Math.random() * 100}%;
              animation-delay: {Math.random() * 5}s;
              animation-duration: {15 + Math.random() * 10}s;
              background: rgba(147, 51, 234, {0.2 + Math.random() * 0.3});
              box-shadow: 0 0 20px rgba(147, 51, 234, 0.3);
            "
				></div>
			{/each}
		</div>
	</section>

	<!-- Main Features Section -->
	<section id="main-features" class="px-4 py-24 sm:px-6 lg:px-8">
		<div class="mx-auto max-w-7xl">
			<div class="grid gap-8 md:grid-cols-2 lg:grid-cols-4">
				{#each mainFeatures as feature, index}
					<div
						class="group relative transition-all duration-700
                     {mainFeaturesVisible
							? 'translate-y-0 opacity-100'
							: 'translate-y-10 opacity-0'}"
						style="transition-delay: {index * 150}ms"
					>
						<div
							class="relative overflow-hidden rounded-2xl border border-purple-500/20 bg-gradient-to-br
                         from-gray-900 to-black p-8
                         transition-all duration-500 hover:scale-105
                         hover:transform hover:border-purple-500/50 hover:shadow-[0_0_30px_rgba(147,51,234,0.3)]"
						>
							<!-- Background gradient -->
							<div
								class="absolute inset-0 bg-gradient-to-br opacity-0 transition-opacity duration-500
                           group-hover:opacity-10 {feature.gradient}"
							></div>

							<!-- Icon -->
							<div
								class="relative mb-6 flex h-16 w-16 transform
                           items-center justify-center rounded-xl bg-purple-600/20 transition-transform
                           duration-500 group-hover:scale-110"
							>
								<svg
									class="h-8 w-8 text-purple-400"
									fill="none"
									stroke="currentColor"
									viewBox="0 0 24 24"
								>
									<path
										stroke-linecap="round"
										stroke-linejoin="round"
										stroke-width="2"
										d={feature.icon}
									></path>
								</svg>
							</div>

							<!-- Content -->
							<h3 class="relative mb-3 text-xl font-bold text-white">
								{feature.title}
							</h3>
							<p class="relative text-gray-400">
								{feature.description}
							</p>

							<!-- Subtle hover glow -->
							<div
								class="pointer-events-none absolute inset-0 opacity-0 transition-opacity duration-500
                           group-hover:opacity-100"
							>
								<div class="absolute inset-0 rounded-2xl border border-purple-500/20"></div>
							</div>
						</div>
					</div>
				{/each}
			</div>
		</div>
	</section>

	<!-- Stats Section -->
	<section id="stats-section" class="px-4 py-24 sm:px-6 lg:px-8">
		<div class="mx-auto max-w-7xl">
			<div class="grid grid-cols-2 gap-8 md:grid-cols-4">
				{#each stats as stat, index}
					<div
						class="text-center transition-all duration-700
                     {statsVisible ? 'translate-y-0 opacity-100' : 'translate-y-10 opacity-0'}"
						style="transition-delay: {index * 100}ms"
					>
						<div
							class="mb-2 bg-gradient-to-r from-purple-400 to-blue-400 bg-clip-text text-4xl
                         font-bold text-transparent md:text-5xl"
						>
							{stat.number}
						</div>
						<div class="text-gray-400">{stat.label}</div>
					</div>
				{/each}
			</div>
		</div>
	</section>
</div>

<style>
	@keyframes pulse {
		0%,
		100% {
			opacity: 0.3;
			transform: scale(1);
		}
		50% {
			opacity: 0.5;
			transform: scale(1.05);
		}
	}

	@keyframes float-random {
		0%,
		100% {
			transform: translate(0, 0) rotate(0deg);
		}
		25% {
			transform: translate(10px, -20px) rotate(90deg);
		}
		50% {
			transform: translate(-10px, -40px) rotate(180deg);
		}
		75% {
			transform: translate(15px, -15px) rotate(270deg);
		}
	}

	@keyframes gradient {
		0% {
			background-position: 0% 50%;
		}
		50% {
			background-position: 100% 50%;
		}
		100% {
			background-position: 0% 50%;
		}
	}

	.animate-gradient {
		background-size: 200% auto;
		animation: gradient 5s linear infinite;
	}

	.animate-float-random {
		animation: float-random 15s ease-in-out infinite;
	}
</style>
