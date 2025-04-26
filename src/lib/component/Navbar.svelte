<script lang="ts">
  import { onMount } from 'svelte';
  
  let isOpen = false;
  let scrolled = false;
  
  function toggleMenu() {
    isOpen = !isOpen;
  }
  
  function handleScroll() {
    scrolled = window.scrollY > 20;
  }
  
  onMount(() => {
    window.addEventListener('scroll', handleScroll);
    return () => window.removeEventListener('scroll', handleScroll);
  });
  
  const navLinks = [
    { name: 'Home', href: '/' },
    { name: 'Services', href: '#services' },
    { name: 'Features', href: '/features' },
    { name: 'About', href: '/about' }
  ];
</script>

<nav 
  class="fixed w-full z-50 transition-all duration-300
         {scrolled ? 'py-2 bg-black' : 'py-4 bg-transparent'}"
>
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    <div class="flex items-center justify-between">
      <!-- Logo -->
      <a href="/" class="flex items-center space-x-2">
        <div class="relative">
          <span class="text-2xl font-bold bg-gradient-to-r bg-white bg-clip-text text-transparent">
            SK8
          </span>
          <div class="absolute -bottom-1 left-0 w-full h-0.5 bg-gray-300"></div>
        </div>
        <span class="text-sm text-gray-300">CONSIGN</span>
      </a>
      
      <!-- Desktop Navigation -->
      <div class="hidden md:flex items-center space-x-8">
        {#each navLinks as link}
          <a 
            href={link.href}
            class="text-gray-300 hover:text-white transition-colors duration-200 relative group"
          >
            {link.name}
            <span class="absolute -bottom-0 left-0 w-0 h-0.5 bg-gray-300 transition-all duration-300 group-hover:w-full"></span>
          </a>
        {/each}
        
        
      </div>
      
      <!-- Mobile menu button -->
      <button 
        on:click={toggleMenu}
        class="md:hidden p-2 text-gray-400 hover:text-white transition-colors"
      >
        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          {#if isOpen}
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
          {:else}
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
          {/if}
        </svg>
      </button>
    </div>
  </div>
  
  <!-- Mobile menu -->
  {#if isOpen}
    <div class="md:hidden bg-black">
      <div class="px-2 pt-2 pb-3 space-y-1">
        {#each navLinks as link}
          <a 
            href={link.href}
            class="block px-3 py-2 text-gray-300 hover:text-white hover:bg-gray-900 rounded-md transition-colors"
          >
            {link.name}
          </a>
        {/each}
      </div>
    </div>
  {/if}
</nav>