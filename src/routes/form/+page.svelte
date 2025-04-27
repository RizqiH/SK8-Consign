<script lang="ts">
    import { onMount } from 'svelte';
    
    let formVisible = false;
    
    // Form data
    let formData = {
      fullName: '',
      email: '',
      phone: '',
      productName: '',
      productCategory: '',
      condition: '',
      purchaseYear: '',
      description: '',
      expectedPrice: '',
      images: []
    };
    
    // Product categories
    const categories = [
      'Console Gaming',
      'PC Gaming',
      'Handheld Gaming',
      'Gaming Accessories',
      'Gaming Merchandise',
      'Retro Gaming',
      'Lainnya'
    ];
    
    // Condition options
    const conditions = [
      'Brand New',
      'Like New',
      'Mint',
      'Excellent',
      'Good',
      'Fair'
    ];
    
    function handleSubmit() {
      // Validasi form
      if (!formData.fullName || !formData.email || !formData.phone || !formData.productName) {
        alert('Mohon lengkapi data yang diperlukan');
        return;
      }
      
      // Simulasi pengiriman form
      const message = encodeURIComponent(
        `Halo, saya ${formData.fullName} ingin mengajukan consign:\n\n` +
        `Produk: ${formData.productName}\n` +
        `Kategori: ${formData.productCategory}\n` +
        `Kondisi: ${formData.condition}\n` +
        `Harga Harapan: ${formData.expectedPrice}\n\n` +
        `Kontak:\n` +
        `Email: ${formData.email}\n` +
        `Telepon: ${formData.phone}\n\n` +
        `Deskripsi: ${formData.description}`
      );
      
      window.open(`https://wa.me/6281217810896?text=${message}`, '_blank');
    }
    
    function handleImageUpload(event) {
      const files = event.target.files;
      if (files) {
        formData.images = Array.from(files);
      }
    }
    
    onMount(() => {
      const observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
          if (entry.isIntersecting) {
            formVisible = true;
          }
        });
      }, { threshold: 0.1 });
      
      const formSection = document.getElementById('consign-form');
      if (formSection) {
        observer.observe(formSection);
      }
      
      return () => {
        if (formSection) {
          observer.unobserve(formSection);
        }
      };
    });
  </script>
  
  <div class="bg-black text-white min-h-screen">
    <!-- Header Section -->
    <section class="relative py-20 overflow-hidden">
      <div class="absolute inset-0 opacity-20">
        <div 
          class="absolute inset-0 bg-gradient-to-br from-purple-900/30 to-black"
          style="background-image: linear-gradient(to right, rgba(147, 51, 234, 0.1) 1px, transparent 1px),
                 linear-gradient(to bottom, rgba(147, 51, 234, 0.1) 1px, transparent 1px);
                 background-size: 40px 40px;">
        </div>
      </div>
      
      <div class="relative z-10 max-w-4xl mx-auto px-4 text-center">
        <h1 class="text-4xl md:text-5xl font-bold mb-6">
          Consignkan barang anda
        </h1>
        <p class="text-xl text-gray-400">
          Isi formulir di bawah ini untuk memulai proses Consign
        </p>
      </div>
    </section>
    
    <!-- Form Section -->
    <section 
      id="consign-form"
      class="py-16 px-4 relative"
    >
      <div 
        class="max-w-3xl mx-auto bg-gray-900 rounded-xl p-8 transition-all duration-700
               {formVisible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'}"
      >
        <form on:submit|preventDefault={handleSubmit} class="space-y-6">
          <!-- Personal Information -->
          <div class="space-y-4">
            <h2 class="text-2xl font-bold text-white mb-4">Informasi Pribadi</h2>
            
            <div>
              <label for="fullName" class="block text-sm font-medium text-gray-300 mb-2">
                Nama Lengkap *
              </label>
              <input 
                type="text" 
                id="fullName"
                bind:value={formData.fullName}
                class="w-full px-4 py-3 rounded-lg bg-black border border-gray-700 text-white 
                       focus:outline-none focus:border-purple-500 transition-colors"
                required
              />
            </div>
            
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
              <div>
                <label for="email" class="block text-sm font-medium text-gray-300 mb-2">
                  Email *
                </label>
                <input 
                  type="email" 
                  id="email"
                  bind:value={formData.email}
                  class="w-full px-4 py-3 rounded-lg bg-black border border-gray-700 text-white 
                         focus:outline-none focus:border-purple-500 transition-colors"
                  required
                />
              </div>
              
              <div>
                <label for="phone" class="block text-sm font-medium text-gray-300 mb-2">
                  Nomor Telepon *
                </label>
                <input 
                  type="tel" 
                  id="phone"
                  bind:value={formData.phone}
                  class="w-full px-4 py-3 rounded-lg bg-black border border-gray-700 text-white 
                         focus:outline-none focus:border-purple-500 transition-colors"
                  required
                />
              </div>
            </div>
          </div>
          
          <!-- Product Information -->
          <div class="space-y-4 pt-6 border-t border-gray-800">
            <h2 class="text-2xl font-bold text-white mb-4">Informasi Produk</h2>
            
            <div>
              <label for="productName" class="block text-sm font-medium text-gray-300 mb-2">
                Nama Produk *
              </label>
              <input 
                type="text" 
                id="productName"
                bind:value={formData.productName}
                class="w-full px-4 py-3 rounded-lg bg-black border border-gray-700 text-white 
                       focus:outline-none focus:border-purple-500 transition-colors"
                required
              />
            </div>
            
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
              <div>
                <label for="productCategory" class="block text-sm font-medium text-gray-300 mb-2">
                  Kategori Produk *
                </label>
                <select 
                  id="productCategory"
                  bind:value={formData.productCategory}
                  class="w-full px-4 py-3 rounded-lg bg-black border border-gray-700 text-white 
                         focus:outline-none focus:border-purple-500 transition-colors"
                  required
                >
                  <option value="">Pilih Kategori</option>
                  {#each categories as category}
                    <option value={category}>{category}</option>
                  {/each}
                </select>
              </div>
              
              <div>
                <label for="condition" class="block text-sm font-medium text-gray-300 mb-2">
                  Kondisi *
                </label>
                <select 
                  id="condition"
                  bind:value={formData.condition}
                  class="w-full px-4 py-3 rounded-lg bg-black border border-gray-700 text-white 
                         focus:outline-none focus:border-purple-500 transition-colors"
                  required
                >
                  <option value="">Pilih Kondisi</option>
                  {#each conditions as condition}
                    <option value={condition}>{condition}</option>
                  {/each}
                </select>
              </div>
            </div>
            
            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
              <div>
                <label for="purchaseYear" class="block text-sm font-medium text-gray-300 mb-2">
                  Tahun Pembelian
                </label>
                <input 
                  type="number" 
                  id="purchaseYear"
                  bind:value={formData.purchaseYear}
                  class="w-full px-4 py-3 rounded-lg bg-black border border-gray-700 text-white 
                         focus:outline-none focus:border-purple-500 transition-colors"
                  min="1990"
                  max={new Date().getFullYear()}
                />
              </div>
              
              <div>
                <label for="expectedPrice" class="block text-sm font-medium text-gray-300 mb-2">
                  Harga Harapan
                </label>
                <div class="relative">
                  <span class="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400">Rp</span>
                  <input 
                    type="text" 
                    id="expectedPrice"
                    bind:value={formData.expectedPrice}
                    class="w-full pl-10 pr-4 py-3 rounded-lg bg-black border border-gray-700 text-white 
                           focus:outline-none focus:border-purple-500 transition-colors"
                    placeholder="0"
                  />
                </div>
              </div>
            </div>
            
            <div>
              <label for="description" class="block text-sm font-medium text-gray-300 mb-2">
                Deskripsi Produk
              </label>
              <textarea 
                id="description"
                bind:value={formData.description}
                rows="4"
                class="w-full px-4 py-3 rounded-lg bg-black border border-gray-700 text-white 
                       focus:outline-none focus:border-purple-500 transition-colors"
                placeholder="Deskripsikan kondisi produk, kelengkapan, dan informasi lainnya..."
              ></textarea>
            </div>
            
            <div>
              <label class="block text-sm font-medium text-gray-300 mb-2">
                Foto Produk
              </label>
              <div class="flex items-center justify-center w-full">
                <label 
                  for="images" 
                  class="flex flex-col items-center justify-center w-full h-32 border-2 border-gray-700 
                         border-dashed rounded-lg cursor-pointer bg-black hover:bg-gray-900 
                         transition-all duration-300"
                >
                  <div class="flex flex-col items-center justify-center pt-5 pb-6">
                    <svg class="w-8 h-8 mb-3 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M15 13l-3-3m0 0l-3 3m3-3v12"></path>
                    </svg>
                    <p class="mb-2 text-sm text-gray-400">
                      <span class="font-semibold">Click to upload</span> or drag and drop
                    </p>
                    <p class="text-xs text-gray-500">PNG, JPG or JPEG (MAX. 5MB)</p>
                  </div>
                  <input 
                    id="images" 
                    type="file" 
                    class="hidden" 
                    accept="image/*"
                    multiple
                    on:change={handleImageUpload}
                  />
                </label>
              </div>
            </div>
          </div>
          
          <!-- Submit Button -->
          <div class="pt-6">
            <button 
              type="submit"
              class="w-full px-6 py-4 bg-purple-600 hover:bg-purple-700 text-white font-semibold 
                     rounded-lg transition-colors duration-300 text-lg"
            >
              Ajukan Consign
            </button>
            <p class="mt-4 text-sm text-gray-400 text-center">
              Dengan mengajukan Consign, Anda menyetujui 
              <a href="#" class="text-purple-400 hover:text-purple-300">syarat dan ketentuan</a> kami.
            </p>
          </div>
        </form>
      </div>
    </section>
  </div>
  
  <style>
    /* Chrome, Safari, Edge, Opera */
    input::-webkit-outer-spin-button,
    input::-webkit-inner-spin-button {
      -webkit-appearance: none;
      margin: 0;
    }
  
    /* Firefox */
    input[type=number] {
      -moz-appearance: textfield;
    }
  </style>