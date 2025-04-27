<script lang="ts">
	import { onMount } from 'svelte';

	let paymentPosition = 0;
	let shippingPosition = 0;
	let partnerSection: HTMLElement;
	let isVisible = false;

	interface PartnerLogo {
		name: string;
		image: string;
	}

	const paymentLogos: PartnerLogo[] = [
		{
			name: 'BNI',
			image: 'https://images.seeklogo.com/logo-png/45/1/bank-bri-logo-png_seeklogo-459990.png'
		},
		{
			name: 'BRIVA',
			image: 'https://images.seeklogo.com/logo-png/45/1/bank-bri-logo-png_seeklogo-459990.png'
		},
		{
			name: 'Mandiri',
			image: 'https://images.seeklogo.com/logo-png/45/1/bank-bri-logo-png_seeklogo-459990.png'
		},
		{
			name: 'BSI',
			image: 'https://images.seeklogo.com/logo-png/45/1/bank-bri-logo-png_seeklogo-459990.png'
		},
		{
			name: 'Bank BJB',
			image: 'https://images.seeklogo.com/logo-png/45/1/bank-bri-logo-png_seeklogo-459990.png'
		},
		{
			name: 'Bank Sampoerna',
			image: 'https://images.seeklogo.com/logo-png/45/1/bank-bri-logo-png_seeklogo-459990.png'
		},
		{
			name: 'Bank DKI',
			image: 'https://images.seeklogo.com/logo-png/45/1/bank-bri-logo-png_seeklogo-459990.png'
		},
		{
			name: 'Bank Jatim',
			image: 'https://images.seeklogo.com/logo-png/45/1/bank-bri-logo-png_seeklogo-459990.png'
		}
	];

	const shippingLogos: PartnerLogo[] = [
		{
			name: 'JNT Express',
			image: 'https://www.kargomurah.co.id/wp-content/uploads/2019/09/Cek-Barang-JNE-1.jpg'
		},
		{
			name: 'SiCepat',
			image: 'https://www.kargomurah.co.id/wp-content/uploads/2019/09/Cek-Barang-JNE-1.jpg'
		},
		{
			name: 'Anteraja',
			image: 'https://www.kargomurah.co.id/wp-content/uploads/2019/09/Cek-Barang-JNE-1.jpg'
		},
		{
			name: 'ID Express',
			image: 'https://www.kargomurah.co.id/wp-content/uploads/2019/09/Cek-Barang-JNE-1.jpg'
		},
		{
			name: 'Ninja Xpress',
			image: 'https://www.kargomurah.co.id/wp-content/uploads/2019/09/Cek-Barang-JNE-1.jpg'
		},
		{
			name: 'POS Indonesia',
			image: 'https://www.kargomurah.co.id/wp-content/uploads/2019/09/Cek-Barang-JNE-1.jpg'
		},
		{
			name: 'JNE',
			image: 'https://www.kargomurah.co.id/wp-content/uploads/2019/09/Cek-Barang-JNE-1.jpg'
		},
		{
			name: 'TIKI',
			image: 'https://www.kargomurah.co.id/wp-content/uploads/2019/09/Cek-Barang-JNE-1.jpg'
		}
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

		if (partnerSection) {
			observer.observe(partnerSection);
		}

		// Auto slide payment to the right
		const paymentInterval = setInterval(() => {
			paymentPosition -= 0.5; // Slower speed for smoother animation
			if (paymentPosition <= -200 * paymentLogos.length) {
				paymentPosition = 0;
			}
		}, 16);

		// Auto slide shipping to the left
		const shippingInterval = setInterval(() => {
			shippingPosition += 0.5;
			if (shippingPosition >= 0) {
				shippingPosition = -200 * shippingLogos.length;
			}
		}, 16);

		return () => {
			if (partnerSection) {
				observer.unobserve(partnerSection);
			}
			clearInterval(paymentInterval);
			clearInterval(shippingInterval);
		};
	});
</script>

<div
	bind:this={partnerSection}
	class="relative w-full bg-gradient-to-b from-black via-purple-900/20 to-black"
>
	<!-- Neon grid background effect -->
	<div
		class="absolute inset-0 opacity-20"
		style="background-image: linear-gradient(to right, rgba(147, 51, 234, 0.1) 1px, transparent 1px),
             linear-gradient(to bottom, rgba(147, 51, 234, 0.1) 1px, transparent 1px);
             background-size: 40px 40px;"
	></div>

	<!-- Payment Section -->
	<div class="relative px-4 py-16 sm:px-6 lg:px-8">
		<div class="mx-auto max-w-7xl">
			<h2
				class="mb-12 text-center text-3xl font-bold text-white transition-all duration-700 sm:text-4xl
                 {isVisible ? 'translate-y-0 opacity-100' : 'translate-y-10 opacity-0'}"
				style="text-shadow: 0 0 10px rgba(147, 51, 234, 0.7), 0 0 20px rgba(147, 51, 234, 0.5);"
			>
				PAYMENT
			</h2>

			<div class="relative overflow-hidden">
				<!-- Gradient fade effect on edges -->
				<div
					class="absolute inset-y-0 left-0 z-10 w-32 bg-gradient-to-r from-black to-transparent"
				></div>
				<div
					class="absolute inset-y-0 right-0 z-10 w-32 bg-gradient-to-l from-black to-transparent"
				></div>

				<div
					class="flex transition-transform duration-0"
					style="transform: translateX({paymentPosition}px)"
				>
					{#each [...paymentLogos, ...paymentLogos] as logo}
						<div class="mx-4 w-48 flex-shrink-0">
							<div
								class="flex h-24 items-center justify-center rounded-lg
                         border border-purple-500/30 bg-black/50
                         p-4 backdrop-blur-sm transition-all duration-300 hover:scale-105
                         hover:transform hover:border-purple-500/60 hover:shadow-[0_0_15px_rgba(147,51,234,0.3)]"
							>
								<img
									src={logo.image}
									alt={logo.name}
									class="max-h-12 max-w-full object-contain opacity-80 transition-opacity hover:opacity-100"
								/>
							</div>
						</div>
					{/each}
				</div>
			</div>
		</div>
	</div>

	<!-- Divider with neon effect -->
	<div class="relative h-px w-full">
		<div
			class="absolute inset-0 bg-gradient-to-r from-transparent via-purple-500/50 to-transparent"
		></div>
	</div>

	<!-- Shipping Section -->
	<div class="relative px-4 py-16 sm:px-6 lg:px-8">
		<div class="mx-auto max-w-7xl">
			<h2
				class="mb-12 text-center text-3xl font-bold text-white transition-all duration-700 sm:text-4xl
                 {isVisible ? 'translate-y-0 opacity-100' : 'translate-y-10 opacity-0'}"
				style="text-shadow: 0 0 10px rgba(59, 130, 246, 0.7), 0 0 20px rgba(59, 130, 246, 0.5);"
			>
				SHIPPING
			</h2>

			<div class="relative overflow-hidden">
				<!-- Gradient fade effect on edges -->
				<div
					class="absolute inset-y-0 left-0 z-10 w-32 bg-gradient-to-r from-black to-transparent"
				></div>
				<div
					class="absolute inset-y-0 right-0 z-10 w-32 bg-gradient-to-l from-black to-transparent"
				></div>

				<div
					class="flex transition-transform duration-0"
					style="transform: translateX({shippingPosition}px)"
				>
					{#each [...shippingLogos, ...shippingLogos] as logo}
						<div class="mx-4 w-48 flex-shrink-0">
							<div
								class="flex h-24 items-center justify-center rounded-lg
                         border border-blue-500/30 bg-black/50
                         p-4 backdrop-blur-sm transition-all duration-300 hover:scale-105
                         hover:transform hover:border-blue-500/60 hover:shadow-[0_0_15px_rgba(59,130,246,0.3)]"
							>
								<img
									src={logo.image}
									alt={logo.name}
									class="max-h-12 max-w-full object-contain opacity-80 transition-opacity hover:opacity-100"
								/>
							</div>
						</div>
					{/each}
				</div>
			</div>
		</div>
	</div>

	<!-- Glowing particles effect -->
	<div class="pointer-events-none absolute inset-0 overflow-hidden">
		{#each Array(20) as _, i}
			<div
				class="animate-float absolute h-1 w-1 rounded-full"
				style="
            left: {Math.random() * 100}%;
            top: {Math.random() * 100}%;
            animation-delay: {Math.random() * 5}s;
            background-color: {i % 2 === 0 ? 'rgba(147, 51, 234, 0.5)' : 'rgba(59, 130, 246, 0.5)'};
            box-shadow: 0 0 10px {i % 2 === 0
					? 'rgba(147, 51, 234, 0.5)'
					: 'rgba(59, 130, 246, 0.5)'};
          "
			></div>
		{/each}
	</div>
</div>

<style>
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
