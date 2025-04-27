<script lang="ts">
	import { onMount } from 'svelte';

	let heroVisible = false;
	let storyVisible = false;
	let teamVisible = false;
	let valuesVisible = false;

	const values = [
		{
			icon: 'M9 12l2 2 4-4m5.618-4.016A11.955 11.955 0 0112 2.944a11.955 11.955 0 01-8.618 3.04A12.02 12.02 0 003 9c0 5.591 3.824 10.29 9 11.622 5.176-1.332 9-6.03 9-11.622 0-1.042-.133-2.052-.382-3.016z',
			title: 'Trust & Security',
			description: 'Keamanan transaksi adalah prioritas utama kami'
		},
		{
			icon: 'M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z',
			title: 'Fast Service',
			description: 'Proses cepat dan efisien untuk semua layanan'
		},
		{
			icon: 'M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0zm6 3a2 2 0 11-4 0 2 2 0 014 0zM7 10a2 2 0 11-4 0 2 2 0 014 0z',
			title: 'Community',
			description: 'Komunitas gamer yang aktif dan suportif'
		}
	];

	onMount(() => {
		const observerOptions = { threshold: 0.1 };

		const heroObserver = new IntersectionObserver((entries) => {
			entries.forEach((entry) => {
				if (entry.isIntersecting) heroVisible = true;
			});
		}, observerOptions);

		const storyObserver = new IntersectionObserver((entries) => {
			entries.forEach((entry) => {
				if (entry.isIntersecting) storyVisible = true;
			});
		}, observerOptions);

		const teamObserver = new IntersectionObserver((entries) => {
			entries.forEach((entry) => {
				if (entry.isIntersecting) teamVisible = true;
			});
		}, observerOptions);

		const valuesObserver = new IntersectionObserver((entries) => {
			entries.forEach((entry) => {
				if (entry.isIntersecting) valuesVisible = true;
			});
		}, observerOptions);

		const heroSection = document.getElementById('hero-section');
		const storySection = document.getElementById('story-section');
		const teamSection = document.getElementById('team-section');
		const valuesSection = document.getElementById('values-section');

		if (heroSection) heroObserver.observe(heroSection);
		if (storySection) storyObserver.observe(storySection);
		if (teamSection) teamObserver.observe(teamSection);
		if (valuesSection) valuesObserver.observe(valuesSection);

		return () => {
			heroObserver.disconnect();
			storyObserver.disconnect();
			teamObserver.disconnect();
			valuesObserver.disconnect();
		};
	});
</script>

<div class="bg-black text-white">
	<!-- Hero Section -->
	<section
		id="hero-section"
		class="relative flex min-h-[60vh] items-center justify-center overflow-hidden"
	>
		<!-- Background gradient and grid -->
		<div
			class="absolute inset-0 bg-gradient-to-br from-black via-purple-900/20 to-black opacity-90"
		></div>
		<div
			class="absolute inset-0 opacity-20"
			style="background-image: linear-gradient(to right, rgba(147, 51, 234, 0.1) 1px, transparent 1px),
             linear-gradient(to bottom, rgba(147, 51, 234, 0.1) 1px, transparent 1px);
             background-size: 40px 40px;"
		></div>

		<!-- Content -->
		<div
			class="relative z-10 px-4 text-center transition-all duration-700
                {heroVisible ? 'translate-y-0 opacity-100' : 'translate-y-10 opacity-0'}"
		>
			<h1 class="text-shadow-neon-purple mb-6 text-5xl font-bold md:text-6xl">
				Tentang SK8 Consign
			</h1>
			<p class="mx-auto max-w-2xl text-xl text-gray-300">
				Platform terpercaya untuk jual beli barang gaming dengan layanan konsinyasi profesional
			</p>
		</div>

		<!-- Floating particles -->
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
	</section>

	<!-- Our Story Section -->
	<section id="story-section" class="relative overflow-hidden px-4 py-24 sm:px-6 lg:px-8">
		<div class="mx-auto max-w-7xl">
			<div class="grid items-center gap-12 md:grid-cols-2">
				<div
					class="transition-all duration-700
                    {storyVisible ? 'translate-x-0 opacity-100' : '-translate-x-10 opacity-0'}"
				>
					<h2 class="mb-6 text-3xl font-bold text-white">Cerita Kami</h2>
					<p class="mb-4 text-gray-300">
						SK8 Consign lahir dari passion kami terhadap gaming dan kebutuhan akan platform jual
						beli yang aman dan terpercaya. Dimulai pada tahun 2020, kami berkomitmen untuk menjadi
						jembatan antara penjual dan pembeli dalam komunitas gaming.
					</p>
					<p class="mb-4 text-gray-300">
						Dengan pengalaman bertahun-tahun dalam industri gaming, kami memahami kebutuhan para
						gamer akan marketplace yang tidak hanya menyediakan produk berkualitas, tetapi juga
						memberikan pengalaman transaksi yang aman dan nyaman.
					</p>
					<p class="text-gray-300">
						Hingga saat ini, kami telah melayani ribuan transaksi dengan tingkat kepuasan pelanggan
						yang tinggi, menjadikan SK8 Consign sebagai destinasi utama untuk jual beli barang
						gaming di Indonesia.
					</p>
				</div>
				<div
					class="relative transition-all duration-700
                    {storyVisible ? 'translate-x-0 opacity-100' : 'translate-x-10 opacity-0'}"
				>
					<div class="relative h-96 overflow-hidden rounded-lg">
						<div
							class="absolute inset-0 transform bg-cover bg-center transition-transform duration-500 hover:scale-105"
							style="background-image: url('https://static.vecteezy.com/system/resources/previews/042/960/123/non_2x/ai-generated-gamepad-in-the-hands-of-a-gamer-on-a-technological-background-neon-lighting-video-games-online-with-friends-winnings-prizes-fun-entertainment-youth-culture-virtual-reality-photo.jpg');"
						></div>
						<div class="absolute inset-0 bg-gradient-to-t from-black/60 to-transparent"></div>
					</div>
					<!-- Neon border effect -->
					<div
						class="absolute -inset-1 rounded-lg bg-gradient-to-r from-purple-600 to-blue-600 opacity-25 blur"
					></div>
				</div>
			</div>
		</div>
	</section>

	<!-- Values Section -->
	<section
		id="values-section"
		class="bg-gradient-to-b from-black via-purple-900/10 to-black px-4 py-24 sm:px-6 lg:px-8"
	>
		<div class="mx-auto max-w-7xl">
			<h2
				class="mb-16 text-center text-3xl font-bold text-white transition-all duration-700
                 {valuesVisible ? 'translate-y-0 opacity-100' : 'translate-y-10 opacity-0'}"
			>
				Nilai-Nilai Kami
			</h2>
			<div class="grid gap-8 md:grid-cols-3">
				{#each values as value, index}
					<div
						class="group relative transition-all duration-700
                   {valuesVisible ? 'translate-y-0 opacity-100' : 'translate-y-10 opacity-0'}"
						style="transition-delay: {index * 150}ms"
					>
						<div
							class="h-full rounded-lg border border-purple-500/30 bg-black/50
                       p-8 transition-all duration-300 hover:scale-105
                       hover:transform hover:border-purple-500/60 hover:shadow-[0_0_15px_rgba(147,51,234,0.3)]"
						>
							<!-- Icon -->
							<div
								class="mx-auto mb-6 flex h-16 w-16 items-center justify-center rounded-lg bg-purple-600/20
                         transition-colors duration-300 group-hover:bg-purple-600/30"
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
										d={value.icon}
									></path>
								</svg>
							</div>
							<h3 class="mb-4 text-center text-xl font-bold text-white">{value.title}</h3>
							<p class="text-center text-gray-400">{value.description}</p>
						</div>
					</div>
				{/each}
			</div>
		</div>
	</section>
</div>

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
