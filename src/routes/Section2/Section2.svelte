<script lang="ts">
	// Interface untuk data benefit card
	interface BenefitCard {
	  id: number;
	  title: string;
	  subtitle: string;
	  description: string;
	  imagePath: string;
	  imageAlt: string;
	}
  
	// Data untuk benefit cards
	const benefitCards: BenefitCard[] = [
	  {
		id: 1,
		title: 'Dijamin',
		subtitle: 'Cepat Laku',
		description: 'Barang yang dijual di SK8 Consign dijamin laku dalam sekejap! *',
		imagePath: '/images/SK2.jpg', // Gambar jalan raya dengan light trail
		imageAlt: 'Jalanan dengan cahaya kendaraan di malam hari'
	  },
	  {
		id: 2,
		title: 'Pencairan Dana',
		subtitle: 'Mudah & Cepat',
		description: 'Uang kamu dijamin cepat cair tanpa ribet.',
		imagePath: 'https://images-cdn.ubuy.co.id/63700bd736f1a7682743980e-npet-k80-tkl-tenkeyless-gaming-keyboard.jpg', // Gambar ATM dengan lampu neon
		imageAlt: 'Mesin ATM dengan pencahayaan neon'
	  },
	  {
		id: 3,
		title: 'Customer Service',
		subtitle: 'Siap Siaga',
		description: 'CS kami siap melayani seluruh kendala dan membantu para pelanggan.',
		imagePath: 'https://www.apple.com/newsroom/images/2024/09/apple-debuts-iphone-16-pro-and-iphone-16-pro-max/tile/Apple-iPhone-16-Pro-hero-geo-240909-lp.jpg.og.jpg?202503102049', // Gambar customer service
		imageAlt: 'Customer service dengan laptop'
	  }
	];
  
	// Animasi on scroll dengan Intersection Observer
	import { onMount } from 'svelte';
  
	let sectionWrapper: HTMLElement;
	let benefitsSection: HTMLElement;
	let titleElement: HTMLElement;
	let cardElements: HTMLElement[] = [];
	let cardContainers: HTMLElement[] = [];
	let glowOverlay: HTMLElement;
	let borderLines: HTMLElement;
	let isVisible = false;
	let particlesContainer: HTMLElement;
	let cardScenes: HTMLElement[] = [];
	
	// Membuat partikel efek untuk latar belakang
	function createParticles() {
	  if (!particlesContainer) return;
	  
	  const particleCount = 40;
	  particlesContainer.innerHTML = '';
	  
	  for (let i = 0; i < particleCount; i++) {
		const particle = document.createElement('div');
		
		// Random properties
		const size = Math.random() * 2 + 1;
		const posX = Math.random() * 100;
		const posY = Math.random() * 100;
		const delay = Math.random() * 3;
		const duration = Math.random() * 10 + 10;
		const opacity = Math.random() * 0.3 + 0.1;
		
		// Random 3D position
		const posZ = Math.random() * 100 - 50;
		
		// Apply styles
		particle.className = 'absolute rounded-full bg-purple-500';
		particle.style.width = `${size}px`;
		particle.style.height = `${size}px`;
		particle.style.left = `${posX}%`;
		particle.style.top = `${posY}%`;
		particle.style.opacity = `${opacity}`;
		particle.style.transform = `translateZ(${posZ}px)`;
		particle.style.animation = `float-particle-3d ${duration}s ease-in-out ${delay}s infinite`;
		
		if (size > 1.5) {
		  particle.style.boxShadow = `0 0 ${size * 2}px rgba(147, 51, 234, 0.6)`;
		}
		
		particlesContainer.appendChild(particle);
	  }
	}
  
	function handleScroll() {
	  if (sectionWrapper) {
		const scrollPosition = window.scrollY;
		const windowHeight = window.innerHeight;
		const sectionTop = sectionWrapper.getBoundingClientRect().top + scrollPosition;
		
		// Section benefits hanya akan naik ketika user scroll ke arahnya
		if (scrollPosition > 0) {
		  // Persentase scroll relatif terhadap posisi section (0-1)
		  const scrollPercentage = Math.min(
			1, 
			Math.max(0, (scrollPosition - (sectionTop - windowHeight)) / (windowHeight * 0.7))
		  );
		  
		  // Transformasi untuk section wrapper - dengan rotasi 3D
		  const translateY = -(scrollPercentage * 150);
		  const rotateX = scrollPercentage * 3; // Sedikit rotasi untuk efek 3D
		  sectionWrapper.style.transform = `translateY(${translateY}px) perspective(1000px) rotateX(${rotateX}deg)`;
		  
		  // Animasi border radius berdasarkan scroll
		  const borderRadius = 40 + (scrollPercentage * 20);
		  sectionWrapper.style.borderRadius = `${borderRadius}px ${borderRadius}px 0 0`;
		  
		  // Animasi glow overlay berdasarkan scroll
		  if (glowOverlay) {
			const glowOpacity = 0.1 + (scrollPercentage * 0.3);
			glowOverlay.style.opacity = glowOpacity.toString();
		  }
		  
		  // Animasi border lines berdasarkan scroll
		  if (borderLines) {
			const lineWidth = 30 + (scrollPercentage * 70);
			borderLines.style.width = `${lineWidth}%`;
		  }
		} else {
		  // Reset posisi jika di atas halaman
		  sectionWrapper.style.transform = 'translateY(0) perspective(1000px) rotateX(0deg)';
		  sectionWrapper.style.borderRadius = '40px 40px 0 0';
		}
	  }
	  
	  // 3D Parallax effect untuk cards
	  if (isVisible && cardElements.length) {
		const scrollPosition = window.scrollY;
		
		cardElements.forEach((card, index) => {
		  const speed = 0.05 + (index * 0.01);
		  const yPos = -(scrollPosition * speed);
		  const zPos = scrollPosition * speed * 0.2;
		  const rotate = (index - 1) * 2; // Different rotation for each card
		  
		  card.style.transform = `translateY(${yPos}px) translateZ(${zPos}px) rotateY(${rotate}deg)`;
		});
	  }
	}
	
	// Animasi hover 3D untuk cards
	function setupCardAnimations() {
	  cardContainers.forEach((container, containerIndex) => {
		const card = cardElements[containerIndex];
		if (!card) return;
		
		const image = card.querySelector('.card-image') as HTMLElement;
		const content = card.querySelector('.card-content') as HTMLElement;
		const title = card.querySelector('.card-title') as HTMLElement;
		const subtitle = card.querySelector('.card-subtitle') as HTMLElement;
		const description = card.querySelector('.card-description') as HTMLElement;
		const scene = cardScenes[containerIndex];
		
		if (!image || !content || !title || !subtitle || !description || !scene) return;
		
		// 3D tilt effect on mouse move
		container.addEventListener('mousemove', (e) => {
		  if (!card) return;
		  
		  const { left, top, width, height } = container.getBoundingClientRect();
		  const x = (e.clientX - left) / width;
		  const y = (e.clientY - top) / height;
		  
		  // Calculate rotation based on mouse position
		  const rotateY = (x - 0.5) * 20; // -10 to +10 degrees
		  const rotateX = (y - 0.5) * -20; // +10 to -10 degrees (inverted)
		  
		  // Apply transformation to the card
		  card.style.transform = `perspective(1000px) rotateX(${rotateX}deg) rotateY(${rotateY}deg) scale3d(1.05, 1.05, 1.05)`;
		  
		  // Parallax effect for elements inside the card
		  const moveX = (x - 0.5) * 20;
		  const moveY = (y - 0.5) * 20;
		  
		  content.style.transform = `translate3d(${moveX}px, ${moveY}px, 30px)`;
		  image.style.transform = `translate3d(${-moveX * 0.3}px, ${-moveY * 0.3}px, 0) scale(1.1)`;
		  
		  // Dynamic shadow based on tilt
		  card.style.boxShadow = `
			${-rotateY}px ${-rotateX * 0.5}px 20px rgba(0, 0, 0, 0.4),
			0 10px 30px rgba(147, 51, 234, 0.2)
		  `;
		  
		  // Light reflection effect
		  const glareX = x * 100;
		  const glareY = y * 100;
		  content.style.backgroundImage = `
			radial-gradient(
			  circle at ${glareX}% ${glareY}%, 
			  rgba(255, 255, 255, 0.1) 0%, 
			  rgba(255, 255, 255, 0) 60%
			)
		  `;
		});
		
		// Reset on mouse leave
		container.addEventListener('mouseleave', () => {
		  if (!card) return;
		  
		  // Smooth transition back to original position
		  card.style.transform = 'perspective(1000px) rotateX(0) rotateY(0) scale3d(1, 1, 1)';
		  card.style.boxShadow = '0 10px 30px rgba(0, 0, 0, 0.3)';
		  content.style.transform = 'translate3d(0, 0, 0)';
		  image.style.transform = 'translate3d(0, 0, 0) scale(1)';
		  content.style.backgroundImage = 'none';
		});
		
		// Extra highlight on click
		container.addEventListener('mousedown', () => {
		  if (!card) return;
		  card.style.transform = 'perspective(1000px) rotateX(0) rotateY(0) scale3d(0.98, 0.98, 0.98)';
		  card.style.boxShadow = '0 5px 15px rgba(0, 0, 0, 0.2)';
		});
		
		container.addEventListener('mouseup', (e) => {
		  if (!card) return;
		  
		  // Return to hover state after click
		  const { left, top, width, height } = container.getBoundingClientRect();
		  const x = (e.clientX - left) / width;
		  const y = (e.clientY - top) / height;
		  
		  const rotateY = (x - 0.5) * 20;
		  const rotateX = (y - 0.5) * -20;
		  
		  card.style.transform = `perspective(1000px) rotateX(${rotateX}deg) rotateY(${rotateY}deg) scale3d(1.05, 1.05, 1.05)`;
		  card.style.boxShadow = `
			${-rotateY}px ${-rotateX * 0.5}px 20px rgba(0, 0, 0, 0.4),
			0 10px 30px rgba(147, 51, 234, 0.2)
		  `;
		});
	  });
	}
	
	// Animated 3D title
	function setupTitleAnimation() {
	  if (!titleElement) return;
	  
	  const title = "Kenapa Harus SK8 Consign?";
	  titleElement.innerHTML = '';
	  
	  title.split('').forEach((char, index) => {
		const span = document.createElement('span');
		span.textContent = char === ' ' ? '\u00A0' : char;
		span.style.animationDelay = `${index * 50}ms`;
		span.className = 'inline-block opacity-0 title-char';
		
		// 3D rotating effect for each character
		span.style.transform = 'rotateY(90deg) translateZ(20px)';
		span.style.transformOrigin = '50% 50%';
		
		titleElement.appendChild(span);
	  });
	}
  
	onMount(() => {
	  const observer = new IntersectionObserver(
		(entries) => {
		  entries.forEach((entry) => {
			if (entry.isIntersecting) {
			  isVisible = true;
			  
			  // Capture all card elements after they're rendered
			  cardElements = Array.from(document.querySelectorAll('.benefit-card'));
			  cardContainers = Array.from(document.querySelectorAll('.card-container'));
			  cardScenes = Array.from(document.querySelectorAll('.card-scene'));
			  
			  // Setup 3D card animations
			  setupCardAnimations();
			}
		  });
		},
		{ threshold: 0.1 }
	  );
  
	  if (benefitsSection) {
		observer.observe(benefitsSection);
	  }
  
	  // Setup 3D particles
	  createParticles();
	  
	  // Setup 3D title animation
	  setupTitleAnimation();
	  
	  // Setup parallax scroll handling
	  window.addEventListener('scroll', handleScroll);
	  // Trigger initial scroll handler
	  handleScroll();
  
	  return () => {
		if (benefitsSection) {
		  observer.unobserve(benefitsSection);
		}
		window.removeEventListener('scroll', handleScroll);
		
		// Remove event listeners from cards
		cardContainers.forEach(container => {
		  container.removeEventListener('mousemove', () => {});
		  container.removeEventListener('mouseleave', () => {});
		  container.removeEventListener('mousedown', () => {});
		  container.removeEventListener('mouseup', () => {});
		});
	  };
	});
  </script>
  
  <div
	bind:this={sectionWrapper}
	class="relative z-30 transform transition-transform duration-500 ease-out will-change-transform"
	style="border-radius: 40px 40px 0 0; margin-top: 0; transform-style: preserve-3d; perspective: 1000px;"
  >
	<!-- Border lines animation element -->
	<div class="absolute top-0 left-1/2 transform -translate-x-1/2 -translate-y-1/2 z-10">
	  <div bind:this={borderLines} class="h-1 bg-gradient-to-r from-transparent via-purple-500 to-transparent transition-all duration-700" style="width: 30%;"></div>
	</div>
	
	<!-- Glow overlay element -->
	<div bind:this={glowOverlay} class="absolute inset-0 rounded-t-[40px] bg-purple-900/10 transition-opacity duration-700 pointer-events-none opacity-10"></div>
	
	<!-- 3D floating elements -->
	<div class="absolute w-20 h-20 top-40 right-[15%] rounded-lg opacity-30 bg-purple-500 animate-float-3d" style="transform: translateZ(50px) rotateX(45deg) rotateY(45deg);"></div>
	<div class="absolute w-16 h-16 bottom-60 left-[10%] rounded-full opacity-20 bg-blue-500 animate-float-3d-reverse" style="transform: translateZ(30px) rotateX(30deg) rotateY(60deg);"></div>
	<div class="absolute w-12 h-12 top-1/2 left-[25%] rounded-lg opacity-25 bg-pink-500 animate-float-3d" style="transform: translateZ(80px) rotateX(60deg) rotateY(30deg);"></div>
	
	<!-- Main section -->
	<section
	  bind:this={benefitsSection}
	  class="rounded-t-[40px] bg-black px-4 py-24 shadow-2xl sm:px-6 lg:px-8 relative overflow-hidden"
	  style="transform-style: preserve-3d; perspective: 2000px;"
	>
	  <!-- 3D perspective container for particles -->
	  <div class="absolute inset-0 perspective-1000 transform-style-preserve-3d">
		<!-- Particles background with 3D effect -->
		<div bind:this={particlesContainer} class="absolute inset-0 pointer-events-none transform-style-preserve-3d" style="transform: translateZ(0px);"></div>
	  </div>
	  
	  <!-- Rotating gradient background -->
	  <div class="absolute inset-0 bg-gradient-to-br from-purple-900/30 via-black to-blue-900/20 animate-gradient-bg pointer-events-none"></div>
	  
	  <!-- Grid pattern background with 3D depth -->
	  <div 
		class="absolute inset-0 opacity-5 pointer-events-none"
		style="background-image: linear-gradient(to right, rgba(147, 51, 234, 0.8) 1px, transparent 1px),
			  linear-gradient(to bottom, rgba(147, 51, 234, 0.8) 1px, transparent 1px);
			  background-size: 30px 30px;
			  transform: translateZ(5px);"
	  ></div>
	  
	  <div class="mx-auto max-w-7xl relative z-10" style="transform-style: preserve-3d;">
		<h2
		  bind:this={titleElement}
		  class="mb-16 text-center text-4xl font-bold text-gray-100 transition-all duration-700 transform-style-preserve-3d {isVisible
			? 'translate-y-0 opacity-100'
			: 'translate-y-10 opacity-0'}"
		  style="transform-style: preserve-3d;"
		>
		  <!-- Title content will be populated by JS for 3D character animation -->
		</h2>
  
		<div class="grid grid-cols-1 gap-8 md:grid-cols-3" style="transform-style: preserve-3d;">
		  {#each benefitCards as card, index}
			<div class="card-container relative perspective-1000" style="transform-style: preserve-3d;">
			  <!-- 3D Scene -->
			  <div class="card-scene relative" bind:this={cardScenes[index]} style="transform-style: preserve-3d;">
				<div
				  class="benefit-card relative transform overflow-hidden rounded-lg shadow-lg transition-all duration-500 {isVisible
					? 'translate-y-0 opacity-100'
					: 'translate-y-20 opacity-0'}"
				  style="transition-delay: {index * 150}ms; transform-style: preserve-3d; backface-visibility: hidden;"
				>
				  <!-- Card glow effect -->
				  <div class="absolute inset-0 opacity-0 group-hover:opacity-60 transition-opacity duration-500 bg-gradient-to-br from-purple-500/20 via-transparent to-blue-500/20 pointer-events-none"></div>
				  
				  <!-- Animated 3D border -->
				  <div class="absolute inset-0 rounded-lg overflow-hidden pointer-events-none" style="transform: translateZ(2px);">
					<div class="absolute inset-0 opacity-0 hover:opacity-100 transition-opacity duration-500">
					  <div class="absolute -inset-[2px] rounded-lg">
						<div class="h-full w-full animate-border-flow rounded-lg"></div>
					  </div>
					</div>
				  </div>
				  
				  <!-- Card image -->
				  <div class="relative h-72 overflow-hidden" style="transform-style: preserve-3d;">
					<div
					  class="card-image absolute inset-0 h-full w-full bg-cover bg-center transition-all duration-500"
					  style="background-image: url('{card.imagePath}'); transform: translateZ(-10px);"
					  aria-label={card.imageAlt}
					></div>
					
					<!-- Gradient overlay for better text readability -->
					<div class="absolute inset-0 bg-gradient-to-t from-black via-black/60 to-transparent" style="transform: translateZ(-5px);"></div>
  
					<!-- Card content positioned over the image with 3D effect -->
					<div class="card-content absolute inset-0 flex flex-col justify-end p-6 text-white transition-all duration-500" style="transform: translateZ(20px); transform-style: preserve-3d;">
					  <h3 class="card-title text-3xl font-bold transition-all duration-300" style="transform: translateZ(10px);">{card.title}</h3>
					  <h4 class="card-subtitle mb-3 text-3xl font-bold transition-all duration-300" style="transform: translateZ(15px);">{card.subtitle}</h4>
					  <p class="card-description text-white text-opacity-90 transform transition-all duration-500" style="transform: translateZ(5px);">{card.description}</p>
					</div>
				  </div>
				</div>
			  </div>
			</div>
		  {/each}
		</div>
	  </div>
	</section>
  </div>
  
  <style>
	@keyframes float-particle-3d {
	  0%, 100% { transform: translate3d(0, 0, 0) rotateX(0deg) rotateY(0deg); }
	  25% { transform: translate3d(30px, -30px, 20px) rotateX(30deg) rotateY(20deg); }
	  50% { transform: translate3d(-20px, -50px, -10px) rotateX(-20deg) rotateY(40deg); }
	  75% { transform: translate3d(10px, -20px, 30px) rotateX(10deg) rotateY(-30deg); }
	}
	
	@keyframes gradient-bg {
	  0% { transform: rotate(0deg); }
	  100% { transform: rotate(360deg); }
	}
	
	@keyframes float-3d {
	  0%, 100% { transform: translate3d(0, 0, 50px) rotateX(45deg) rotateY(45deg); }
	  50% { transform: translate3d(20px, -20px, 70px) rotateX(60deg) rotateY(30deg); }
	}
	
	@keyframes float-3d-reverse {
	  0%, 100% { transform: translate3d(0, 0, 30px) rotateX(30deg) rotateY(60deg); }
	  50% { transform: translate3d(-15px, -25px, 50px) rotateX(45deg) rotateY(20deg); }
	}
	
	@keyframes title-char {
	  0% { transform: rotateY(90deg) translateZ(20px); opacity: 0; }
	  20% { opacity: 1; }
	  100% { transform: rotateY(0) translateZ(0); opacity: 1; }
	}
	
	@keyframes border-flow {
	  0%, 100% { 
		background-image: conic-gradient(
		  from 0deg,
		  transparent 0%,
		  rgba(147, 51, 234, 0.8) 25%,
		  rgba(59, 130, 246, 0.8) 50%,
		  rgba(147, 51, 234, 0.8) 75%,
		  transparent 100%
		);
	  }
	  50% { 
		background-image: conic-gradient(
		  from 180deg,
		  transparent 0%,
		  rgba(147, 51, 234, 0.8) 25%,
		  rgba(59, 130, 246, 0.8) 50%,
		  rgba(147, 51, 234, 0.8) 75%,
		  transparent 100%
		);
	  }
	}
	
	.animate-border-flow {
	  animation: border-flow 4s linear infinite;
	}
	
	.animate-gradient-bg {
	  background-size: 200% 200%;
	  animation: gradient-bg 20s linear infinite;
	}
	
	.title-char {
	  animation: title-char 0.8s ease-out forwards;
	}
	
	.will-change-transform {
	  will-change: transform;
	}
	
	.perspective-1000 {
	  perspective: 1000px;
	}
	
	.transform-style-preserve-3d {
	  transform-style: preserve-3d;
	}
  </style>