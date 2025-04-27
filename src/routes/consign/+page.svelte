<script lang="ts">
    import { onMount } from 'svelte';
    
    let heroVisible = false;
    let stepsVisible = false;
    let benefitsVisible = false;
    let ctaVisible = false;
    let shopVisible = false;
    
    const consignedItems = [
      {
        id: 1,
        name: 'PlayStation 5 Digital Edition',
        condition: 'Like New',
        price: 'Rp 7.500.000',
        image: 'https://datascripmall.id/media/webp_image/catalog/product/cache/95a5307f46190cd7a50cf0819ebeb220/3/_/3_166.webp',
        status: 'Available',
        consignDate: '2 hari yang lalu'
      },
      {
        id: 2,
        name: 'Xbox Series X',
        condition: 'Mint',
        price: 'Rp 8.200.000',
        image: 'https://www.static-src.com/wcsstore/Indraprastha/images/catalog/full//107/MTA-29892085/no_brand_microsoft_xbox_series_x_halo_infinite_limited_edition_console_bundle_full02_jklh1bvo.jpg',
        status: 'Available',
        consignDate: '3 hari yang lalu'
      },
      {
        id: 3,
        name: 'Nintendo Switch OLED',
        condition: 'Like New',
        price: 'Rp 4.800.000',
        image: 'https://media.dinomarket.com/docs/imgTD/2021-09/NInOLED_5_220921220927_ll.jpg.jpg',
        status: 'Sold',
        consignDate: '5 hari yang lalu'
      },
      {
        id: 4,
        name: 'ROG Ally Gaming Handheld',
        condition: 'Good',
        price: 'Rp 9.500.000',
        image: 'https://www.asus.com/us/site/gaming/assets/images/rog/ROG-Ally/hero.webp',
        status: 'Available',
        consignDate: '1 hari yang lalu'
      },
      {
        id: 5,
        name: 'Steam Deck 512GB',
        condition: 'Like New',
        price: 'Rp 8.900.000',
        image: 'https://www.gmp-electric.com/cdn/shop/files/b97680ae-df5a-435e-9b3a-c342754b1a89_800x.jpg?v=1737611905',
        status: 'Available',
        consignDate: '4 hari yang lalu'
      },
      {
        id: 6,
        name: 'Gaming PC RTX 3080',
        condition: 'Excellent',
        price: 'Rp 18.500.000',
        image: 'https://images.tokopedia.net/img/cache/500-square/VqbcmM/2020/10/10/0d3f278a-564f-4fe1-889d-e6d6ce7c5332.jpg',
        status: 'Reserved',
        consignDate: '6 hari yang lalu'
      }
    ];
    
    const consignSteps = [
      {
        number: '01',
        title: 'Submit Barang',
        description: 'Kirim detail barang gaming Anda melalui form online kami atau datang langsung ke store.',
        icon: 'M12 6v6m0 0v6m0-6h6m-6 0H6'
      },
      {
        number: '02',
        title: 'Verifikasi & Pengecekan',
        description: 'Tim ahli kami akan memeriksa kondisi dan keaslian barang Anda secara menyeluruh.',
        icon: 'M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z'
      },
      {
        number: '03',
        title: 'Penilaian Harga',
        description: 'Kami akan memberikan penilaian harga yang fair berdasarkan kondisi dan nilai pasar.',
        icon: 'M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z'
      },
      {
        number: '04',
        title: 'Proses Penjualan',
        description: 'Setelah deal, barang akan dipasarkan melalui platform kami dengan eksposur maksimal.',
        icon: 'M13 10V3L4 14h7v7l9-11h-7z'
      }
    ];
    
    const benefits = [
      {
        title: 'Harga Terbaik',
        description: 'Dapatkan nilai maksimal untuk barang gaming Anda dengan sistem penilaian yang transparan.',
        icon: 'M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z'
      },
      {
        title: 'Proses Cepat',
        description: 'Dari submit hingga terjual, kami pastikan prosesnya cepat dan efisien.',
        icon: 'M13 10V3L4 14h7v7l9-11h-7z'
      },
      {
        title: 'Aman & Terpercaya',
        description: 'Transaksi aman dengan dokumentasi lengkap dan proses yang profesional.',
        icon: 'M9 12l2 2 4-4m5.618-4.016A11.955 11.955 0 0112 2.944a11.955 11.955 0 01-8.618 3.04A12.02 12.02 0 003 9c0 5.591 3.824 10.29 9 11.622 5.176-1.332 9-6.03 9-11.622 0-1.042-.133-2.052-.382-3.016z'
      }
    ];
    
    onMount(() => {
      const observerOptions = { threshold: 0.1 };
      
      const observers = [
        { element: 'hero-section', callback: () => heroVisible = true },
        { element: 'shop-section', callback: () => shopVisible = true },
        { element: 'steps-section', callback: () => stepsVisible = true },
        { element: 'benefits-section', callback: () => benefitsVisible = true },
        { element: 'cta-section', callback: () => ctaVisible = true }
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
      class="relative min-h-screen flex items-center justify-center overflow-hidden"
    >
      <!-- Background effects -->
      <div class="absolute inset-0">
        <div class="absolute inset-0 bg-gradient-to-br from-black via-purple-900/30 to-black"></div>
        <div 
          class="absolute inset-0 opacity-20"
          style="background-image: linear-gradient(to right, rgba(147, 51, 234, 0.1) 1px, transparent 1px),
                 linear-gradient(to bottom, rgba(147, 51, 234, 0.1) 1px, transparent 1px);
                 background-size: 40px 40px;"
        ></div>
      </div>
      
      <!-- Content -->
      <div class="relative z-10 text-center px-4 transition-all duration-1000 
                  {heroVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'}">
        <h1 class="text-6xl md:text-7xl font-bold mb-6">
          <span class="bg-gradient-to-r from-purple-400 via-pink-500 to-cyan-400 bg-clip-text text-transparent">
            Consign
          </span>
          <br />
          <span class="text-white">Barang Gaming Anda</span>
        </h1>
        <p class="text-xl text-gray-300 max-w-3xl mx-auto mb-12">
          Maksimalkan nilai barang gaming Anda dengan layanan konsinyasi profesional kami.
          Proses mudah, aman, dan menguntungkan.
        </p>
        
        <!-- Animated CTA Button -->
      <a 
      href="/form"
      class="relative inline-block group"
    >
      <div class="absolute -inset-1 bg-gradient-to-r from-purple-600 to-pink-600 rounded-lg blur opacity-75 
                  group-hover:opacity-100 transition duration-1000 group-hover:duration-200"></div>
      <button class="relative px-12 py-4 bg-black rounded-lg leading-none flex items-center">
        <span class="text-2xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-purple-400 to-pink-400 
                     animate-text-shimmer">
          Mau Consign ?
        </span>
      </button>
    </a>
      </div>
      
      <!-- Floating particles -->
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
    
    <!-- Shop/Marketplace Section -->
    <section 
      id="shop-section"
      class="py-24 px-4 sm:px-6 lg:px-8 bg-black"
    >
      <div class="max-w-7xl mx-auto">
        <div class="text-center mb-16 transition-all duration-700 
                   {shopVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'}">
          <h2 class="text-4xl font-bold mb-4 text-white">
            Consigned Items
          </h2>
          <p class="text-xl text-gray-400">Barang" berkualitas yang sedang diSK8 Consign</p>
        </div>
        
        <!-- Items Grid -->
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
          {#each consignedItems as item, index}
            <div 
              class="group relative bg-gray-900 rounded-lg overflow-hidden 
                     transition-all duration-300 hover:translate-y-[-4px]
                     {shopVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'}"
              style="transition-delay: {index * 100}ms;"
            >
              <!-- Image Container -->
              <div class="relative aspect-[4/3] overflow-hidden">
                <div 
                  class="absolute inset-0 bg-cover bg-center"
                  style="background-image: url('{item.image}');"
                ></div>
                
                <!-- Status Badge -->
                <div class="absolute top-4 right-4">
                  <span class="px-3 py-1 rounded-full text-xs font-medium
                             {item.status === 'Available' ? 'bg-green-500 text-white' : 
                              item.status === 'Sold' ? 'bg-red-500 text-white' : 
                              'bg-yellow-500 text-white'}">
                    {item.status}
                  </span>
                </div>
              </div>
              
              <!-- Item Details -->
              <div class="p-6">
                <h3 class="text-lg font-semibold text-white mb-2">
                  {item.name}
                </h3>
                
                <div class="flex items-center justify-between mb-4">
                  <span class="text-sm text-gray-400">
                    Kondisi: {item.condition}
                  </span>
                  <span class="text-sm text-gray-500">
                    {item.consignDate}
                  </span>
                </div>
                
                <div class="flex items-center justify-between pt-4 border-t border-gray-800">
                  <span class="text-xl font-bold text-white">
                    {item.price}
                  </span>
                  
                  {#if item.status === 'Available'}
                    <button 
                      class="px-4 py-2 bg-purple-600 hover:bg-purple-700 text-white text-sm font-medium rounded transition-colors"
                      on:click={() => {
                        const message = encodeURIComponent(`Halo, saya tertarik dengan ${item.name} (${item.condition}) seharga ${item.price}`);
                        window.open(`https://wa.me/628123456789?text=${message}`, '_blank');
                      }}
                    >
                      Beli Sekarang
                    </button>
                  {:else if item.status === 'Reserved'}
                    <button 
                      class="px-4 py-2 bg-gray-700 text-gray-400 text-sm font-medium rounded cursor-not-allowed"
                      disabled
                    >
                      Reserved
                    </button>
                  {:else}
                    <button 
                      class="px-4 py-2 bg-gray-700 text-gray-400 text-sm font-medium rounded cursor-not-allowed"
                      disabled
                    >
                      Sold Out
                    </button>
                  {/if}
                </div>
              </div>
            </div>
          {/each}
        </div>
        
        <!-- View All Button -->
        <div class="text-center mt-16">
          <a href="/shop" 
             class="inline-flex items-center px-6 py-3 border border-gray-700 text-gray-300 hover:text-white hover:border-gray-500 rounded transition-all">
            <span>Lihat Semua Barang</span>
            <svg class="w-4 h-4 ml-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
            </svg>
          </a>
        </div>
      </div>
    </section>
    
    <!-- Steps Section -->
    <section 
      id="steps-section"
      class="py-24 px-4 sm:px-6 lg:px-8"
    >
      <div class="max-w-7xl mx-auto">
        <h2 class="text-4xl font-bold text-center mb-16 transition-all duration-700 
                   {stepsVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'}">
          Cara Consign
        </h2>
        
        <div class="grid md:grid-cols-2 lg:grid-cols-4 gap-8">
          {#each consignSteps as step, index}
            <div 
              class="relative transition-all duration-700 
                     {stepsVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'}"
              style="transition-delay: {index * 150}ms"
            >
              <!-- Connector line -->
              {#if index < consignSteps.length - 1}
                <div class="hidden lg:block absolute top-12 left-1/2 w-full h-0.5 bg-gradient-to-r from-purple-500/50 to-transparent"></div>
              {/if}
              
              <div class="relative p-6 rounded-2xl bg-gradient-to-br from-gray-900 to-black 
                         border border-purple-500/20 hover:border-purple-500/50 
                         transition-all duration-300 group">
                <!-- Step number -->
                <div class="absolute -top-4 -left-4 w-12 h-12 rounded-full bg-purple-600 
                           flex items-center justify-center text-xl font-bold">
                  {step.number}
                </div>
                
                <!-- Icon -->
                <div class="w-16 h-16 mb-6 rounded-lg bg-purple-600/20 
                           flex items-center justify-center group-hover:bg-purple-600/30 
                           transition-colors duration-300">
                  <svg class="w-8 h-8 text-purple-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d={step.icon}></path>
                  </svg>
                </div>
                
                <h3 class="text-xl font-bold mb-3">{step.title}</h3>
                <p class="text-gray-400">{step.description}</p>
              </div>
            </div>
          {/each}
        </div>
      </div>
    </section>
    
    <!-- Benefits Section -->
    <section 
      id="benefits-section"
      class="py-24 px-4 sm:px-6 lg:px-8 bg-gradient-to-b from-black via-purple-900/10 to-black"
    >
      <div class="max-w-7xl mx-auto">
        <h2 class="text-4xl font-bold text-center mb-16 transition-all duration-700 
                   {benefitsVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'}">
          Keuntungan Consign di SK8
        </h2>
        
        <div class="grid md:grid-cols-3 gap-8">
          {#each benefits as benefit, index}
            <div 
              class="p-8 rounded-2xl bg-black/50 border border-purple-500/20 
                     hover:border-purple-500/50 transition-all duration-700 
                     hover:transform hover:scale-105 hover:shadow-[0_0_30px_rgba(147,51,234,0.3)]
                     {benefitsVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'}"
              style="transition-delay: {index * 150}ms"
            >
              <div class="w-16 h-16 mb-6 rounded-xl bg-purple-600/20 
                         flex items-center justify-center">
                <svg class="w-8 h-8 text-purple-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d={benefit.icon}></path>
                </svg>
              </div>
              <h3 class="text-xl font-bold mb-3">{benefit.title}</h3>
              <p class="text-gray-400">{benefit.description}</p>
            </div>
          {/each}
        </div>
      </div>
    </section>
    
    <!-- CTA Section -->
    <section 
      id="cta-section"
      class="py-24 px-4 sm:px-6 lg:px-8"
    >
      <div class="max-w-3xl mx-auto text-center transition-all duration-700 
                  {ctaVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'}">
        <h2 class="text-4xl font-bold mb-6">Siap untuk Consign?</h2>
        <p class="text-xl text-gray-300 mb-12">
          Mulai maksimalkan nilai barang gaming Anda sekarang juga
        </p>
        
        <!-- Animated CTA Button -->
        <a 
          href="#consign-form"
          class="relative inline-block group"
        >
          <div class="absolute -inset-1 bg-gradient-to-r from-purple-600 to-pink-600 rounded-lg blur opacity-75 
                      group-hover:opacity-100 transition duration-1000 group-hover:duration-200 
                      animate-tilt"></div>
          <button class="relative px-16 py-6 bg-black rounded-lg leading-none flex items-center">
            <span class="text-3xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-purple-400 to-pink-400 
                         animate-text-shimmer">
              Mau Consign ?
            </span>
          </button>
        </a>
      </div>
    </section>
  </div>
  
  <style>
    @keyframes float-random {
      0%, 100% { transform: translate(0, 0) rotate(0deg); }
      25% { transform: translate(10px, -20px) rotate(90deg); }
      50% { transform: translate(-10px, -40px) rotate(180deg); }
      75% { transform: translate(15px, -15px) rotate(270deg); }
    }
    
    @keyframes text-shimmer {
      0% { background-position: 0% 50%; }
      50% { background-position: 100% 50%; }
      100% { background-position: 0% 50%; }
    }
    
    @keyframes tilt {
      0%, 100% { transform: rotate(-1deg); }
      50% { transform: rotate(1deg); }
    }
    
    .animate-float-random {
      animation: float-random 15s ease-in-out infinite;
    }
    
    .animate-text-shimmer {
      background-size: 200% auto;
      animation: text-shimmer 3s linear infinite;
    }
    
    .animate-tilt {
      animation: tilt 3s ease-in-out infinite;
    }
  </style>