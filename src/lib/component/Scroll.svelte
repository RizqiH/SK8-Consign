<script lang="ts">
    import { onMount } from 'svelte';
    
    let visible = false;
    
    function scrollToTop() {
      window.scrollTo({
        top: 0,
        behavior: 'smooth'
      });
    }
    
    function handleScroll() {
      visible = window.scrollY > 300;
    }
    
    onMount(() => {
      window.addEventListener('scroll', handleScroll);
      
      return () => {
        window.removeEventListener('scroll', handleScroll);
      };
    });
  </script>
  
  <button
    class="fixed bottom-6 right-6 p-3 rounded-full bg-black text-white shadow-lg z-50 transition-all duration-300 {visible ? 'opacity-100 translate-y-0' : 'opacity-0 translate-y-10'}"
    on:click={scrollToTop}
    aria-label="Scroll ke atas"
    class:hidden={!visible}
  >
    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" fill="none" viewBox="0 0 24 24" stroke="currentColor">
      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 10l7-7m0 0l7 7m-7-7v18"></path>
    </svg>
  </button>
  
  <style>
    .hidden {
      pointer-events: none;
    }
  </style>