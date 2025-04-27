<script lang="ts">
	import { onMount } from 'svelte';

	let parallaxContainer: HTMLElement;
	let background: HTMLElement;
	let content: HTMLElement;
	let computerImage: HTMLElement;
	let joystickImage: HTMLElement;
	let cassetteImage: HTMLElement;

	// Parallax effect dengan mouse movement
	function handleMouseMove(event: MouseEvent) {
		const { clientX, clientY } = event;
		const x = clientX / window.innerWidth;
		const y = clientY / window.innerHeight;

		if (background) {
			background.style.transform = `translateX(${x * -20}px) translateY(${y * -20}px)`;
		}

		if (computerImage) {
			computerImage.style.transform = `translateX(${x * 15}px) translateY(${y * 15}px)`;
		}

		if (joystickImage) {
			joystickImage.style.transform = `translateX(${x * -10}px) translateY(${y * -10}px)`;
		}

		if (cassetteImage) {
			cassetteImage.style.transform = `translateX(${x * 5}px) translateY(${y * 5}px)`;
		}
	}

	function handleScroll() {
		if (parallaxContainer) {
			const scrollPosition = window.scrollY;
			const windowHeight = window.innerHeight;

			// Perubahan opacity untuk content hero
			const contentOpacity = Math.max(0, Math.min(1, 1 - scrollPosition / (windowHeight * 0.5)));
			const contentTranslateY = scrollPosition * 0.4;

			if (content) {
				content.style.opacity = contentOpacity.toString();
				content.style.transform = `translateY(${contentTranslateY}px)`;
			}

			// Parallax untuk gambar-gambar
			if (computerImage) {
				computerImage.style.transform = `translateY(${scrollPosition * 0.2}px)`;
			}

			if (joystickImage) {
				joystickImage.style.transform = `translateY(${scrollPosition * 0.3}px)`;
			}

			if (cassetteImage) {
				cassetteImage.style.transform = `translateY(${scrollPosition * 0.25}px)`;
			}
		}
	}

	onMount(() => {
		window.addEventListener('mousemove', handleMouseMove);
		window.addEventListener('scroll', handleScroll);

		return () => {
			window.removeEventListener('mousemove', handleMouseMove);
			window.removeEventListener('scroll', handleScroll);
		};
	});
</script>

<div
	bind:this={parallaxContainer}
	class="relative h-screen min-h-screen overflow-hidden bg-gradient-to-br from-black to-purple-900"
>
	<!-- Background layer dengan gradients -->
	<div
		bind:this={background}
		class="absolute inset-0 transition-transform duration-200 ease-out"
		style="background: radial-gradient(circle at 30% 50%, rgba(128, 0, 128, 0.3), transparent 40%), 
             radial-gradient(circle at 70% 50%, rgba(0, 0, 255, 0.3), transparent 40%);"
	></div>

	<!-- Retro computer image -->
	<div
		bind:this={computerImage}
		class="absolute inset-0 flex items-center justify-center transition-all duration-300 ease-out"
	>
		<div
			class="h-full w-full max-w-4xl bg-contain bg-center bg-no-repeat opacity-80"
			style="background-image: url('https://static.vecteezy.com/system/resources/previews/042/960/123/non_2x/ai-generated-gamepad-in-the-hands-of-a-gamer-on-a-technological-background-neon-lighting-video-games-online-with-friends-winnings-prizes-fun-entertainment-youth-culture-virtual-reality-photo.jpg');"
		></div>
	</div>

	<!-- Joystick element -->
	<div
		bind:this={joystickImage}
		class="absolute right-20 bottom-40 transition-all duration-300 ease-out"
	>
		<div
			class="h-40 w-40 bg-contain bg-center bg-no-repeat opacity-70"
			style="background-image: url('https://static.vecteezy.com/system/resources/previews/042/960/123/non_2x/ai-generated-gamepad-in-the-hands-of-a-gamer-on-a-technological-background-neon-lighting-video-games-online-with-friends-winnings-prizes-fun-entertainment-youth-culture-virtual-reality-photo.jpg');"
		></div>
	</div>

	<!-- Cassette element -->
	<div
		bind:this={cassetteImage}
		class="absolute bottom-20 left-20 transition-all duration-300 ease-out"
	>
		<div
			class="h-40 w-40 bg-contain bg-center bg-no-repeat opacity-70"
			style="background-image: url('https://static.vecteezy.com/system/resources/previews/042/960/123/non_2x/ai-generated-gamepad-in-the-hands-of-a-gamer-on-a-technological-background-neon-lighting-video-games-online-with-friends-winnings-prizes-fun-entertainment-youth-culture-virtual-reality-photo.jpg');"
		></div>
	</div>

	<!-- Content dengan neon effect -->
	<div
		bind:this={content}
		class="relative z-10 flex h-screen flex-col items-start justify-center px-6 transition-all duration-300 sm:px-12 lg:px-24"
	>
		<div class="max-w-4xl">
			<h1
				class="text-shadow-neon-purple mb-6 text-4xl font-bold text-white sm:text-5xl md:text-6xl"
			>
				Jual Beli Barang Kamu<br />
				<span class="mt-2 block">di SK8 Consign</span>
			</h1>

			<div class="mt-8 flex flex-wrap gap-4">
				<a
					href="#"
					class="inline-flex items-center rounded-full border border-gray-700 bg-black px-6 py-3 text-white transition-colors hover:bg-gray-900"
				>
					<svg
						class="mr-2 h-5 w-5"
						fill="currentColor"
						viewBox="0 0 20 20"
						xmlns="http://www.w3.org/2000/svg"
					>
						<path d="M10 12a2 2 0 100-4 2 2 0 000 4z"></path>
						<path
							fill-rule="evenodd"
							d="M10 18a8 8 0 100-16 8 8 0 000 16zm0-2a6 6 0 100-12 6 6 0 000 12z"
							clip-rule="evenodd"
						></path>
					</svg>
					Play Store
				</a>

				<a
					href="#"
					class="inline-flex items-center rounded-full border border-gray-700 bg-black px-6 py-3 text-white transition-colors hover:bg-gray-900"
				>
					<svg
						class="mr-2 h-5 w-5"
						fill="currentColor"
						viewBox="0 0 20 20"
						xmlns="http://www.w3.org/2000/svg"
					>
						<path d="M10 12a2 2 0 100-4 2 2 0 000 4z"></path>
						<path
							fill-rule="evenodd"
							d="M10 18a8 8 0 100-16 8 8 0 000 16zm0-2a6 6 0 100-12 6 6 0 000 12z"
							clip-rule="evenodd"
						></path>
					</svg>
					App Store
				</a>
			</div>
		</div>
	</div>
</div>

<style>
	.text-shadow-neon-purple {
		text-shadow:
			0 0 10px rgba(147, 51, 234, 0.7),
			0 0 20px rgba(147, 51, 234, 0.5),
			0 0 30px rgba(147, 51, 234, 0.3);
	}
</style>
