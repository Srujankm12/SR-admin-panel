<script>
	import { goto } from "$app/navigation";
  import { page } from '$app/stores';
	import Header from "$lib/header.svelte";
  let isLoading = false;
</script>

    <div class="min-h-screen flex flex-col bg-white text-gray-900">

    <Header />

     <main class="flex-grow container mx-auto px-6 py-28">

      {#if isLoading}
      <div class="flex justify-center items-center h-full">
          <div class="animate-spin h-12 w-12 rounded-full border-t-4 border-gray-800"></div>
      </div>

      {:else}
        <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6">
        <div class="bg-white rounded-lg shadow-lg p-6 flex flex-col items-center justify-between h-64   hover:shadow-xl duration-300 ">
        <h2 class="text-lg font-semibold text-center">Technical Team Report</h2>
        <p class="text-gray-600 text-sm text-center">Monitor daily technical progress and team updates.</p>
            <button 
            class="text-sm py-2 px-4 rounded bg-black text-white font-semibold transition duration-200"
             on:click={()=>goto('/adminlogin/'+$page.params.mainpage+"/reportpage")}
             >View Report  
           </button>
        </div>
        <div class="bg-white rounded-lg shadow-lg p-6 flex flex-col items-center justify-between h-64   hover:shadow-xl duration-300 ">
          <h2 class="text-lg font-semibold text-center">Sales Team Report</h2>
          <p class="text-gray-600 text-sm text-center">View client engagement and daily sales metrics insights.</p>
          <button 
          class="text-sm py-2 px-4 rounded bg-black text-white font-semibold "       
            >View Report</button>
        </div>
      </div>
      {/if}
    </main>
  </div>
  