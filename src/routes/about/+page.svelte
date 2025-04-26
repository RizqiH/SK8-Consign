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
      entries.forEach(entry => {
        if (entry.isIntersecting) heroVisible = true;
      });
    }, observerOptions);
    
    const storyObserver = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) storyVisible = true;
      });
    }, observerOptions);
    
    const teamObserver = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) teamVisible = true;
      });
    }, observerOptions);
    
    const valuesObserver = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
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
    class="relative min-h-[60vh] flex items-center justify-center overflow-hidden"
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
    <div class="relative z-10 text-center px-4 transition-all duration-700 
                {heroVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'}">
      <h1 class="text-5xl md:text-6xl font-bold mb-6 text-shadow-neon-purple">
        Tentang SK8 Consign
      </h1>
      <p class="text-xl text-gray-300 max-w-2xl mx-auto">
        Platform terpercaya untuk jual beli barang gaming dengan layanan konsinyasi profesional
      </p>
    </div>
    
    <!-- Floating particles -->
    <div class="absolute inset-0 pointer-events-none overflow-hidden">
      {#each Array(15) as _, i}
        <div 
          class="absolute w-1 h-1 rounded-full animate-float"
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
  <section 
    id="story-section"
    class="py-24 px-4 sm:px-6 lg:px-8 relative overflow-hidden"
  >
    <div class="max-w-7xl mx-auto">
      <div class="grid md:grid-cols-2 gap-12 items-center">
        <div class="transition-all duration-700 
                    {storyVisible ? 'opacity-100 translate-x-0' : 'opacity-0 -translate-x-10'}">
          <h2 class="text-3xl font-bold mb-6 text-white">
            Cerita Kami
          </h2>
          <p class="text-gray-300 mb-4">
            SK8 Consign lahir dari passion kami terhadap gaming dan kebutuhan akan platform jual beli yang aman dan terpercaya. Dimulai pada tahun 2020, kami berkomitmen untuk menjadi jembatan antara penjual dan pembeli dalam komunitas gaming.
          </p>
          <p class="text-gray-300 mb-4">
            Dengan pengalaman bertahun-tahun dalam industri gaming, kami memahami kebutuhan para gamer akan marketplace yang tidak hanya menyediakan produk berkualitas, tetapi juga memberikan pengalaman transaksi yang aman dan nyaman.
          </p>
          <p class="text-gray-300">
            Hingga saat ini, kami telah melayani ribuan transaksi dengan tingkat kepuasan pelanggan yang tinggi, menjadikan SK8 Consign sebagai destinasi utama untuk jual beli barang gaming di Indonesia.
          </p>
        </div>
        <div class="relative transition-all duration-700 
                    {storyVisible ? 'opacity-100 translate-x-0' : 'opacity-0 translate-x-10'}">
          <div class="relative h-96 rounded-lg overflow-hidden">
            <div 
              class="absolute inset-0 bg-cover bg-center transform hover:scale-105 transition-transform duration-500"
              style="background-image: url('https://static.vecteezy.com/system/resources/previews/042/960/123/non_2x/ai-generated-gamepad-in-the-hands-of-a-gamer-on-a-technological-background-neon-lighting-video-games-online-with-friends-winnings-prizes-fun-entertainment-youth-culture-virtual-reality-photo.jpg');"
            ></div>
            <div class="absolute inset-0 bg-gradient-to-t from-black/60 to-transparent"></div>
          </div>
          <!-- Neon border effect -->
          <div class="absolute -inset-1 bg-gradient-to-r from-purple-600 to-blue-600 rounded-lg blur opacity-25"></div>
        </div>
      </div>
    </div>
  </section>
  
  <!-- Values Section -->
  <section 
    id="values-section"
    class="py-24 px-4 sm:px-6 lg:px-8 bg-gradient-to-b from-black via-purple-900/10 to-black"
  >
    <div class="max-w-7xl mx-auto">
      <h2 class="text-3xl font-bold text-center text-white mb-16 transition-all duration-700 
                 {valuesVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'}">
        Nilai-Nilai Kami
      </h2>
      <div class="grid md:grid-cols-3 gap-8">
        {#each values as value, index}
          <div 
            class="relative group transition-all duration-700 
                   {valuesVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'}"
            style="transition-delay: {index * 150}ms"
          >
            <div class="bg-black/50 p-8 rounded-lg border border-purple-500/30 
                       hover:border-purple-500/60 transition-all duration-300 h-full
                       hover:transform hover:scale-105 hover:shadow-[0_0_15px_rgba(147,51,234,0.3)]">
              <!-- Icon -->
              <div class="w-16 h-16 mb-6 mx-auto bg-purple-600/20 rounded-lg flex items-center justify-center
                         group-hover:bg-purple-600/30 transition-colors duration-300">
                <svg class="w-8 h-8 text-purple-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d={value.icon}></path>
                </svg>
              </div>
              <h3 class="text-xl font-bold text-white mb-4 text-center">{value.title}</h3>
              <p class="text-gray-400 text-center">{value.description}</p>
            </div>
          </div>
        {/each}
      </div>
    </div>
  </section> 
</div>

<style>
  .text-shadow-neon-purple {
    text-shadow: 0 0 10px rgba(147, 51, 234, 0.7), 
                 0 0 20px rgba(147, 51, 234, 0.5), 
                 0 0 30px rgba(147, 51, 234, 0.3);
  }
  
  @keyframes float {
    0%, 100% {
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