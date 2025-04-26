<script lang="ts">
    import { onMount } from 'svelte';
    
    let heroVisible = false;
    let mainFeaturesVisible = false;
    let statsVisible = false;
    
    const mainFeatures = [
      {
        title: 'Smart Authentication',
        description: 'Sistem otentikasi barang yang canggih untuk memastikan keaslian produk gaming yang Anda beli atau jual.',
        icon: 'M9 12l2 2 4-4m5.618-4.016A11.955 11.955 0 0112 2.944a11.955 11.955 0 01-8.618 3.04A12.02 12.02 0 003 9c0 5.591 3.824 10.29 9 11.622 5.176-1.332 9-6.03 9-11.622 0-1.042-.133-2.052-.382-3.016z',
        gradient: 'from-purple-500 to-blue-500'
      },
      {
        title: 'Express Shipping',
        description: 'Layanan pengiriman cepat dengan tracking real-time untuk memastikan barang Anda sampai dengan aman.',
        icon: 'M13 10V3L4 14h7v7l9-11h-7z',
        gradient: 'from-blue-500 to-cyan-500'
      },
      {
        title: 'Secure Payment',
        description: 'Sistem pembayaran yang aman dengan berbagai metode pembayaran untuk kenyamanan transaksi Anda.',
        icon: 'M3 10h18M7 15h1m4 0h1m-7 4h12a3 3 0 003-3V8a3 3 0 00-3-3H6a3 3 0 00-3 3v8a3 3 0 003 3z',
        gradient: 'from-cyan-500 to-purple-500'
      },
      {
        title: '24/7 Support',
        description: 'Tim support yang siap membantu Anda kapan saja untuk menyelesaikan semua kendala transaksi.',
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
        { element: 'hero-section', callback: () => heroVisible = true },
        { element: 'main-features', callback: () => mainFeaturesVisible = true },
        { element: 'stats-section', callback: () => statsVisible = true }
      ];
      
      const intersectionObservers = observers.map(({ element, callback }) => {
        const observer = new IntersectionObserver((entries) => {
          entries.forEach(entry => {
            if (entry.isIntersecting) callback();
          });
        }, observerOptions);
        
        const el = document.getElementById(element);
        if (el) observer.observe(el);
        
        return observer;
      });
      
      return () => {
        intersectionObservers.forEach(observer => observer.disconnect());
      };
    });
  </script>
  
  <div class="bg-black text-white">
    <!-- Hero Section -->
    <section 
      id="hero-section"
      class="relative min-h-[75vh] flex items-center justify-center overflow-hidden"
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
      <div class="relative z-10 text-center px-4 transition-all duration-1000 
                  {heroVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'}">
        <h1 class="text-6xl md:text-7xl font-bold mb-6 bg-gradient-to-r from-purple-400 via-pink-500 to-cyan-400 
                   bg-clip-text text-transparent animate-gradient">
          Powerful Features
        </h1>
        <p class="text-xl text-gray-300 max-w-3xl mx-auto mb-8">
          Temukan fitur-fitur canggih yang membuat SK8 Consign menjadi platform jual beli gaming terdepan di Indonesia
        </p>
        <div class="flex justify-center gap-4">
          <button class="px-8 py-3 bg-gradient-to-r from-purple-600 to-blue-600 rounded-full font-semibold
                       hover:from-purple-700 hover:to-blue-700 transition-all duration-300 transform hover:scale-105">
            Get Started
          </button>
          <button class="px-8 py-3 border border-purple-500/50 rounded-full font-semibold
                       hover:bg-purple-500/10 transition-all duration-300">
            Learn More
          </button>
        </div>
      </div>
      
      <!-- Floating elements -->
      <div class="absolute inset-0 pointer-events-none overflow-hidden">
        {#each Array(20) as _, i}
          <div 
            class="absolute w-2 h-2 rounded-full animate-float-random"
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
    <section 
      id="main-features"
      class="py-24 px-4 sm:px-6 lg:px-8"
    >
      <div class="max-w-7xl mx-auto">
        <div class="grid md:grid-cols-2 lg:grid-cols-4 gap-8">
          {#each mainFeatures as feature, index}
            <div 
              class="relative group transition-all duration-700 
                     {mainFeaturesVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'}"
              style="transition-delay: {index * 150}ms"
            >
              <div class="relative p-8 rounded-2xl bg-gradient-to-br from-gray-900 to-black 
                         border border-purple-500/20 overflow-hidden
                         hover:border-purple-500/50 transition-all duration-500
                         hover:transform hover:scale-105 hover:shadow-[0_0_30px_rgba(147,51,234,0.3)]">
                <!-- Background gradient -->
                <div class="absolute inset-0 opacity-0 group-hover:opacity-10 transition-opacity duration-500
                           bg-gradient-to-br {feature.gradient}"></div>
                
                <!-- Icon -->
                <div class="relative w-16 h-16 mb-6 rounded-xl bg-purple-600/20 
                           flex items-center justify-center transform group-hover:scale-110 
                           transition-transform duration-500">
                  <svg class="w-8 h-8 text-purple-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d={feature.icon}></path>
                  </svg>
                </div>
                
                <!-- Content -->
                <h3 class="relative text-xl font-bold text-white mb-3">
                  {feature.title}
                </h3>
                <p class="relative text-gray-400">
                  {feature.description}
                </p>
                
                <!-- Subtle hover glow -->
                <div class="absolute inset-0 opacity-0 group-hover:opacity-100 transition-opacity duration-500
                           pointer-events-none">
                  <div class="absolute inset-0 border border-purple-500/20 rounded-2xl"></div>
                </div>
              </div>
            </div>
          {/each}
        </div>
      </div>
    </section>
    
  
    <!-- Stats Section -->
    <section 
      id="stats-section"
      class="py-24 px-4 sm:px-6 lg:px-8"
    >
      <div class="max-w-7xl mx-auto">
        <div class="grid grid-cols-2 md:grid-cols-4 gap-8">
          {#each stats as stat, index}
            <div 
              class="text-center transition-all duration-700 
                     {statsVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'}"
              style="transition-delay: {index * 100}ms"
            >
              <div class="text-4xl md:text-5xl font-bold bg-gradient-to-r from-purple-400 to-blue-400 
                         bg-clip-text text-transparent mb-2">
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
      0%, 100% { opacity: 0.3; transform: scale(1); }
      50% { opacity: 0.5; transform: scale(1.05); }
    }
    
    @keyframes float-random {
      0%, 100% { transform: translate(0, 0) rotate(0deg); }
      25% { transform: translate(10px, -20px) rotate(90deg); }
      50% { transform: translate(-10px, -40px) rotate(180deg); }
      75% { transform: translate(15px, -15px) rotate(270deg); }
    }
    
    @keyframes gradient {
      0% { background-position: 0% 50%; }
      50% { background-position: 100% 50%; }
      100% { background-position: 0% 50%; }
    }
    
    .animate-gradient {
      background-size: 200% auto;
      animation: gradient 5s linear infinite;
    }
    
    .animate-float-random {
      animation: float-random 15s ease-in-out infinite;
    }
  </style>