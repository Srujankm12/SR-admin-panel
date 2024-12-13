<script>
  import { goto } from '$app/navigation';
  import { onMount } from 'svelte';

  let email = '';
  let password = '';
  let loading = false;
  let errorMessage = '';
  let showError = false;

  const handleLogin = async () => {
      loading = true;
      showError = false;
      errorMessage = '';

      try {
          const response = await fetch("http://localhost:8000/login", {
              method: "POST",
              body: JSON.stringify({ email, password }),
          });

          if (response.ok) {
              const data = await response.json();
              console.log("Login successful:", data);
              goto("/adminlogin/"+data.message);
          } else {
              const jsonResponse = await response.json();
              errorMessage = jsonResponse.message || 'Invalid email or password.';
              showError = true;
          }
      } catch (error) {
          console.error("Error:", error);
          errorMessage = 'Failed to communicate with the server. Please try again later.';
          showError = true;
      } finally {
          loading = false;
          setTimeout(() => {
              showError = false;
          }, 3000);
      }
  };
</script>


  
  <div class="flex justify-center items-center min-h-screen bg-gradient-to-br from-orange-100 via-white to-orange-50">
    <!-- Login Card -->
    <div class="bg-white shadow-2xl rounded-lg p-8 max-w-md w-full border-t-4 border-orange-500">
      <!-- Logo and Company Name -->
      <div class="text-center mb-8">
        <img src="/logo.jpeg" alt="SR Automation Logo" class="mx-auto w-20 h-20 mb-4" />
        <h1 class="text-4xl font-bold text-gray-800">SR Automation</h1>
        <p class="text-sm text-gray-600 font-medium">
        
        </p>
      </div>
  
      <!-- Login Form -->
      <form on:submit|preventDefault={handleLogin} class="space-y-6">
        <div>
          <label for="email" class="block text-sm font-medium text-gray-700">
            Email Address
          </label>
          <input
            id="email"
            type="email"
            bind:value={email}
            required
            class="mt-1 block w-full px-4 py-3 border border-gray-300 rounded-lg shadow-sm focus:outline-none focus:ring-1 focus:ring-orange-500 focus:border-orange-500 placeholder-gray-400"
            placeholder="Enter your email"
          />
        </div>
        <div>
          <label for="password" class="block text-sm font-medium text-gray-700">
            Password
          </label>
          <input
            id="password"
            type="password"
            bind:value={password}
            required
            class="mt-1 block w-full px-4 py-3 border border-gray-300 rounded-lg shadow-sm focus:outline-none focus:ring-1 focus:ring-orange-500 focus:border-orange-500 placeholder-gray-400"
            placeholder="Enter your password"
          />
        </div>
        <button
          type="submit"
          class="w-full bg-orange-600 text-white font-semibold py-3 px-4 rounded-lg shadow-lg hover:bg-orange-700 focus:outline-none focus:ring focus:ring-orange-300 transition"

        >
          Login
        </button>
      </form>
  
      <!-- Footer -->
      <div class="mt-4 text-center text-sm text-gray-500">
        
      </div>
    </div>
  </div>
  