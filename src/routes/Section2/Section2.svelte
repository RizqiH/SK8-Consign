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
        title: "Dijamin",
        subtitle: "Cepat Laku",
        description: "Barang yang dijual di SK8 Consign dijamin laku dalam sekejap! *",
        imagePath: "/images/SK2.jpg", // Gambar jalan raya dengan light trail
        imageAlt: "Jalanan dengan cahaya kendaraan di malam hari"
      },
      {
        id: 2,
        title: "Pencairan Dana",
        subtitle: "Mudah & Cepat",
        description: "Uang kamu dijamin cepat cair tanpa ribet.",
        imagePath: "/images/atm-neon.jpg", // Gambar ATM dengan lampu neon
        imageAlt: "Mesin ATM dengan pencahayaan neon"
      },
      {
        id: 3,
        title: "Customer Service",
        subtitle: "Siap Siaga",
        description: "CS kami siap melayani seluruh kendala dan membantu para pelanggan.",
        imagePath: "/images/customer-service.jpg", // Gambar customer service
        imageAlt: "Customer service dengan laptop"
      }
    ];
    
    // Animasi on scroll dengan Intersection Observer
    import { onMount } from 'svelte';
    
    let sectionWrapper: HTMLElement;
    let benefitsSection: HTMLElement;
    let isVisible = false;
    
    function handleScroll() {
      if (sectionWrapper) {
        const scrollPosition = window.scrollY;
        const windowHeight = window.innerHeight;
        
        // Section benefits hanya akan naik ketika user mulai scroll
        if (scrollPosition > 0) {
          // Persentase scroll (0-1)
          const scrollPercentage = Math.min(1, scrollPosition / (windowHeight * 0.5));
          
          // Mulai dengan tidak ada translasi, lalu naik hingga -120px maksimum
          const translateY = -(scrollPercentage * 120);
          
          sectionWrapper.style.transform = `translateY(${translateY}px)`;
        } else {
          // Reset posisi jika di atas halaman
          sectionWrapper.style.transform = 'translateY(0)';
        }
      }
    }
    
    onMount(() => {
      const observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
          if (entry.isIntersecting) {
            isVisible = true;
          }
        });
      }, { threshold: 0.1 });
      
      if (benefitsSection) {
        observer.observe(benefitsSection);
      }
      
      // Setup parallax scroll handling
      window.addEventListener('scroll', handleScroll);
      // Trigger initial scroll handler
      handleScroll();
      
      return () => {
        if (benefitsSection) {
          observer.unobserve(benefitsSection);
        }
        window.removeEventListener('scroll', handleScroll);
      };
    });
  </script>
  
  <div
    bind:this={sectionWrapper}
    class="relative z-30 transform transition-transform duration-500 ease-out"
    style="border-radius: 40px 40px 0 0; margin-top: 0;"
  >
    <section 
      bind:this={benefitsSection}
      class="py-24 px-4 sm:px-6 lg:px-8 bg-black rounded-t-[40px] shadow-2xl"
    >
      <div class="max-w-7xl mx-auto">
        <h2 
          class="text-4xl font-bold text-center text-gray-100 mb-16 transition-all duration-700 {isVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'}"
        >
          Kenapa Harus SK8 Consign?
        </h2>
        
        <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
          {#each benefitCards as card, index}
            <div 
              class="relative overflow-hidden rounded-lg shadow-lg transform transition-all duration-700 {isVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-20'}"
              style="transition-delay: {index * 150}ms"
            >
              <!-- Card image -->
              <div class="relative h-72 overflow-hidden">
                <div 
                  class="absolute inset-0 bg-cover bg-center w-full h-full transition-transform duration-500 hover:scale-110"
                  style="background-image: url('{card.imagePath}')"
                  aria-label={card.imageAlt}
                ></div>
                <!-- Dark overlay for better text readability -->
                <div class="absolute inset-0 bg-black bg-opacity-40"></div>
                
                <!-- Card content positioned over the image -->
                <div class="absolute inset-0 flex flex-col justify-end p-6 text-white">
                  <h3 class="text-3xl font-bold">{card.title}</h3>
                  <h4 class="text-3xl font-bold mb-3">{card.subtitle}</h4>
                  <p class="text-white text-opacity-90">{card.description}</p>
                </div>
              </div>
            </div>
          {/each}
        </div>
      </div>
    </section>
  </div>