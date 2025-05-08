<script lang="ts">
	import { onMount } from 'svelte';
  
	let parallaxContainer: HTMLElement;
	let background: HTMLElement;
	let content: HTMLElement;
	let computerImage: HTMLElement;
	let joystickImage: HTMLElement;
	let cassetteImage: HTMLElement;
	let particles: HTMLElement;
	let glowEffects: HTMLElement;
	let neonGrid: HTMLElement;
	
	// Animasi latar belakang dengan particles
	const createParticles = () => {
	  if (!particles) return;
	  
	  const particleCount = 60;
	  particles.innerHTML = '';
	  
	  for (let i = 0; i < particleCount; i++) {
		const particle = document.createElement('div');
		
		// Random properties
		const size = Math.random() * 3 + 1;
		const posX = Math.random() * 100;
		const posY = Math.random() * 100;
		const delay = Math.random() * 10;
		const duration = Math.random() * 30 + 15;
		const opacity = Math.random() * 0.5 + 0.1;
		
		// Apply styles
		particle.className = 'absolute rounded-full';
		particle.style.width = `${size}px`;
		particle.style.height = `${size}px`;
		particle.style.left = `${posX}%`;
		particle.style.top = `${posY}%`;
		particle.style.opacity = `${opacity}`;
		
		// Random color - purple, blue, or pink
		const colorChoice = Math.random();
		if (colorChoice < 0.33) {
		  particle.style.backgroundColor = 'rgba(147, 51, 234, 0.8)'; // Purple
		} else if (colorChoice < 0.66) {
		  particle.style.backgroundColor = 'rgba(59, 130, 246, 0.8)'; // Blue
		} else {
		  particle.style.backgroundColor = 'rgba(236, 72, 153, 0.8)'; // Pink
		}
		
		particle.style.animation = `float-particle ${duration}s ease-in-out ${delay}s infinite`;
		
		// Add glow effect for larger particles
		if (size > 2) {
		  particle.style.boxShadow = `0 0 ${size * 3}px ${particle.style.backgroundColor.replace('0.8', '0.6')}`;
		}
		
		particles.appendChild(particle);
	  }
	};
	
	// Parallax effect dengan mouse movement (enhanced)
	function handleMouseMove(event: MouseEvent) {
	  const { clientX, clientY } = event;
	  const x = clientX / window.innerWidth;
	  const y = clientY / window.innerHeight;
	  
	  // Lebih smooth dengan requestAnimationFrame
	  requestAnimationFrame(() => {
		if (background) {
		  background.style.transform = `translate3d(${x * -35}px, ${y * -35}px, 0) scale(1.05)`;
		}
  
		if (computerImage) {
		  computerImage.style.transform = `translate3d(${x * 25}px, ${y * 25}px, ${x * 10}px) scale(${1 + x * 0.03})`;
		}
  
		if (joystickImage) {
		  joystickImage.style.transform = `translate3d(${x * -18}px, ${y * -18}px, ${y * 5}px) rotate(${x * 8}deg)`;
		}
  
		if (cassetteImage) {
		  cassetteImage.style.transform = `translate3d(${x * 15}px, ${y * 15}px, ${y * -5}px) rotate(${y * -8}deg)`;
		}
		
		if (glowEffects) {
		  glowEffects.style.opacity = (0.4 + x * 0.2 + y * 0.2).toString();
		  glowEffects.style.backgroundPosition = `${50 + x * 15}% ${50 + y * 15}%`;
		}
		
		if (neonGrid) {
		  neonGrid.style.backgroundPosition = `${x * 10}px ${y * 10}px`;
		}
		
		// Parallax for particles
		if (particles) {
		  particles.style.transform = `translate3d(${x * -20}px, ${y * -20}px, 0)`;
		}
	  });
	}
  
	function handleScroll() {
	  if (parallaxContainer) {
		const scrollPosition = window.scrollY;
		const windowHeight = window.innerHeight;
  
		// Improved parallax effect on scroll
		requestAnimationFrame(() => {
		  // Perubahan opacity dan position untuk content hero
		  const contentOpacity = Math.max(0, Math.min(1, 1 - scrollPosition / (windowHeight * 0.4)));
		  const contentTranslateY = scrollPosition * 0.5;
		  const contentScale = 1 - (scrollPosition * 0.0005);
  
		  if (content) {
			content.style.opacity = contentOpacity.toString();
			content.style.transform = `translateY(${contentTranslateY}px) scale(${contentScale})`;
		  }
  
		  // Parallax untuk gambar-gambar dengan efek 3D
		  if (computerImage) {
			const computerTranslateY = scrollPosition * 0.2;
			const computerScale = 1 - (scrollPosition * 0.0004);
			const computerRotateY = scrollPosition * 0.02;
			computerImage.style.transform = `translateY(${computerTranslateY}px) scale(${computerScale}) rotateY(${computerRotateY}deg)`;
		  }
  
		  if (joystickImage) {
			const joystickTranslateY = scrollPosition * 0.35;
			const joystickTranslateX = scrollPosition * 0.1;
			const joystickRotate = scrollPosition * 0.03;
			joystickImage.style.transform = `translate(${joystickTranslateX}px, ${joystickTranslateY}px) rotate(${joystickRotate}deg)`;
		  }
  
		  if (cassetteImage) {
			const cassetteTranslateY = scrollPosition * 0.3;
			const cassetteTranslateX = scrollPosition * -0.1;
			const cassetteRotate = scrollPosition * -0.03;
			cassetteImage.style.transform = `translate(${cassetteTranslateX}px, ${cassetteTranslateY}px) rotate(${cassetteRotate}deg)`;
		  }
		  
		  // Parallax effect for background with depth
		  if (background) {
			const backgroundScale = 1 + (scrollPosition * 0.0008);
			const backgroundTranslateY = scrollPosition * 0.05;
			background.style.transform = `translateY(${backgroundTranslateY}px) scale(${backgroundScale})`;
		  }
		  
		  // Parallax for neon grid
		  if (neonGrid) {
			neonGrid.style.transform = `translateY(${scrollPosition * 0.08}px)`;
		  }
		});
	  }
	}
	
	// Pulsating glow effect
	function animateGlow() {
	  if (!glowEffects) return;
	  
	  const intensity = { value: 0.4 };
	  const increasingIntensity = { value: true };
	  const maxIntensity = 0.7;
	  const minIntensity = 0.4;
	  const step = 0.003;
	  
	  function updateGlow() {
		if (increasingIntensity.value) {
		  intensity.value += step;
		  if (intensity.value >= maxIntensity) {
			increasingIntensity.value = false;
		  }
		} else {
		  intensity.value -= step;
		  if (intensity.value <= minIntensity) {
			increasingIntensity.value = true;
		  }
		}
		
		if (glowEffects) {
		  glowEffects.style.opacity = intensity.value.toString();
		}
		
		requestAnimationFrame(updateGlow);
	  }
	  
	  updateGlow();
	}
  
	onMount(() => {
	  window.addEventListener('mousemove', handleMouseMove);
	  window.addEventListener('scroll', handleScroll);
	  
	  // Initialize effects
	  createParticles();
	  animateGlow();
	  
	  return () => {
		window.removeEventListener('mousemove', handleMouseMove);
		window.removeEventListener('scroll', handleScroll);
	  };
	});
  </script>
  
  <div
	bind:this={parallaxContainer}
	class="relative min-h-screen h-screen overflow-hidden bg-gradient-to-br from-black to-purple-900"
  >
	<!-- Background layer dengan gradients -->
	<div
	  bind:this={background}
	  class="absolute inset-0 transition-transform duration-200 ease-out will-change-transform"
	  style="background: radial-gradient(circle at 30% 50%, rgba(128, 0, 128, 0.4), transparent 45%), 
			 radial-gradient(circle at 70% 50%, rgba(0, 0, 255, 0.4), transparent 45%),
			 radial-gradient(circle at 50% 20%, rgba(236, 72, 153, 0.3), transparent 40%);"
	></div>
	
	<!-- Neon grid overlay -->
	<div 
	  bind:this={neonGrid}
	  class="absolute inset-0 z-5 opacity-15 pointer-events-none will-change-transform"
	  style="background-image: linear-gradient(to right, rgba(147, 51, 234, 0.8) 1px, transparent 1px),
			 linear-gradient(to bottom, rgba(147, 51, 234, 0.8) 1px, transparent 1px);
			 background-size: 40px 40px;"
	></div>
	
	<!-- Pulsating glow effect -->
	<div 
	  bind:this={glowEffects}
	  class="absolute inset-0 z-5 opacity-40 pointer-events-none transition-all duration-1000 ease-out will-change-opacity"
	  style="background: radial-gradient(circle at center, rgba(147, 51, 234, 0.6) 0%, rgba(59, 130, 246, 0.3) 30%, transparent 70%);
			 background-size: 150% 150%;"
	></div>
	
	<!-- Particle layer -->
	<div 
	  bind:this={particles} 
	  class="absolute inset-0 z-10 pointer-events-none overflow-hidden will-change-transform"
	></div>
  
	<!-- Retro computer image -->
	<div
	  bind:this={computerImage}
	  class="absolute inset-0 flex items-center justify-center transition-all duration-300 ease-out will-change-transform"
	>
	  <div
		class="w-full h-full max-w-4xl bg-contain bg-center bg-no-repeat opacity-80"
		style="background-image: url('https://static.vecteezy.com/system/resources/previews/042/960/123/non_2x/ai-generated-gamepad-in-the-hands-of-a-gamer-on-a-technological-background-neon-lighting-video-games-online-with-friends-winnings-prizes-fun-entertainment-youth-culture-virtual-reality-photo.jpg');"
	  ></div>
	</div>
	
	<!-- Joystick element -->
	<div
	  bind:this={joystickImage}
	  class="absolute right-20 bottom-40 transition-all duration-300 ease-out will-change-transform"
	>
	  <div class="relative group">
		<div
		  class="w-40 h-40 bg-contain bg-center bg-no-repeat opacity-70 group-hover:opacity-90 transition-all duration-500"
		  style="background-image: url('https://static.vecteezy.com/system/resources/previews/042/960/123/non_2x/ai-generated-gamepad-in-the-hands-of-a-gamer-on-a-technological-background-neon-lighting-video-games-online-with-friends-winnings-prizes-fun-entertainment-youth-culture-virtual-reality-photo.jpg');"
		></div>
		<!-- Glow effect -->
		<div class="absolute inset-0 -m-4 rounded-full bg-purple-500 opacity-30 blur-xl animate-pulse"></div>
	  </div>
	</div>
  
	<!-- Cassette element -->
	<div
	  bind:this={cassetteImage}
	  class="absolute left-20 bottom-20 transition-all duration-300 ease-out will-change-transform"
	>
	  <div class="relative group">
		<div
		  class="w-40 h-40 bg-contain bg-center bg-no-repeat opacity-70 group-hover:opacity-90 transition-all duration-500"
		  style="background-image: url('https://static.vecteezy.com/system/resources/previews/042/960/123/non_2x/ai-generated-gamepad-in-the-hands-of-a-gamer-on-a-technological-background-neon-lighting-video-games-online-with-friends-winnings-prizes-fun-entertainment-youth-culture-virtual-reality-photo.jpg');"
		></div>
		<!-- Glow effect -->
		<div class="absolute inset-0 -m-4 rounded-full bg-blue-500 opacity-30 blur-xl animate-pulse"></div>
	  </div>
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
			class="group inline-flex items-center rounded-full border border-gray-700 bg-black px-6 py-3 text-white transition-all duration-300 hover:bg-gray-900 hover:border-purple-500 hover:shadow-lg hover:shadow-purple-500/20"
		  >
			<svg
			  class="mr-2 h-5 w-5 transition-transform duration-300 group-hover:rotate-12 group-hover:scale-110"
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
			<span class="relative">
			  <span class="relative z-10">Play Store</span>
			  <span class="absolute -bottom-1 left-0 w-0 h-0.5 bg-purple-500 group-hover:w-full transition-all duration-300"></span>
			</span>
		  </a>
  
		  <a
			href="#"
			class="group inline-flex items-center rounded-full border border-gray-700 bg-black px-6 py-3 text-white transition-all duration-300 hover:bg-gray-900 hover:border-blue-500 hover:shadow-lg hover:shadow-blue-500/20"
		  >
			<svg
			  class="mr-2 h-5 w-5 transition-transform duration-300 group-hover:rotate-12 group-hover:scale-110"
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
			<span class="relative">
			  <span class="relative z-10">App Store</span>
			  <span class="absolute -bottom-1 left-0 w-0 h-0.5 bg-blue-500 group-hover:w-full transition-all duration-300"></span>
			</span>
		  </a>
		</div>
	  </div>
	</div>
  </div>
  
  <style>
	@keyframes float-particle {
	  0%, 100% { transform: translate(0, 0); }
	  25% { transform: translate(30px, -30px); }
	  50% { transform: translate(-20px, -50px); }
	  75% { transform: translate(10px, -20px); }
	}
	
	@keyframes pulse {
	  0%, 100% { opacity: 0.2; transform: scale(1); }
	  50% { opacity: 0.4; transform: scale(1.1); }
	}
	
	.animate-pulse {
	  animation: pulse 4s ease-in-out infinite;
	}
	
	.opacity-15 {
	  opacity: 0.15;
	}
	
	.will-change-transform {
	  will-change: transform;
	}
	
	.will-change-opacity {
	  will-change: opacity;
	}
	
	.text-shadow-neon-purple {
	  text-shadow:
		0 0 10px rgba(147, 51, 234, 0.7),
		0 0 20px rgba(147, 51, 234, 0.5),
		0 0 30px rgba(147, 51, 234, 0.3);
	}
  </style>