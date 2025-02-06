<script>

	import { goto } from "$app/navigation";
  import { page } from '$app/stores';
  export let data;

</script>
    <div class="min-h-screen flex flex-col bg-white text-gray-900">
      <header class="bg-black text-white shadow-md py-3 rounded-b-sm">
        <div class="container mx-auto px-6 flex items-center justify-between">
          <div class="flex items-center space-x-3">
            <img src="/logo.jpeg" alt="SRA BAO Logo" class="w-12 h-12 rounded-full border-2 border-white shadow-sm" />
            <div>
              <h1 class="text-xl font-bold tracking-wide">SRA BAO</h1>
              <p class="text-xs opacity-80">Daily Reporting System</p>
            </div>
          </div>
        </div>
      </header>
    
      <main class="flex-grow container mx-auto px-6 py-10">
        <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6">
          <div class="bg-white rounded-lg shadow-lg p-6 flex flex-col items-center justify-between h-64 border-t-4 border-black hover:shadow-xl hover:scale-105 transition-transform duration-200">
            <h2 class="text-lg font-semibold text-center">Technical Team Report</h2>
            <p class="text-gray-600 text-sm text-center">Monitor daily technical progress and team updates.</p>
            <button 
            class="text-sm py-2 px-4 rounded bg-black text-white font-semibold transition duration-200"
          on:click={()=>goto('/adminlogin/'+$page.params.mainpage+"/reportpage")}
           >
          View Report
        </button>
        </div>
        <div class="bg-white rounded-lg shadow-lg p-6 flex flex-col items-center justify-between h-64 border-t-4 border-black hover:shadow-xl hover:scale-105 transition-transform duration-200">
          <h2 class="text-lg font-semibold text-center">Sales Team Report</h2>
          <p class="text-gray-600 text-sm text-center mb-4">View client engagement and daily sales metrics insights.</p>
          <button 
          class="text-sm py-2 px-4 rounded bg-black text-white font-semibold transition duration-200"          >View Report</button>
        </div>
      </div>
    </main>
  

    <footer class="bg-black text-white py-3 text-center text-sm shadow-inner">
      <p>&copy; 2024 SRA BAO. All Rights Reserved.</p>
    </footer>
  </div>
  