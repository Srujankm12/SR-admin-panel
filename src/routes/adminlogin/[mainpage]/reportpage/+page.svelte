<script>
    import { onMount } from "svelte";

    let data = [];
    let isLoading = true; // Add loading state
    const userId = "6143feaa-1a69-4519-9f4b-47219e81909d"; 
    const apiUrl = `http://localhost:8000/getdata/${userId}`;

    // State for expanded row details
    let expandedRow = null;

    // Fetch data from the API
    onMount(async () => {
        try {
            const response = await fetch(apiUrl);
            if (response.ok) {
                const jsonResponse = await response.json();
                data = Array.isArray(jsonResponse) ? jsonResponse : [jsonResponse]; // Ensure it's an array
            } else {
                console.error("Failed to fetch data:", response.statusText);
            }
        } catch (error) {
            console.error("Error fetching data:", error);
        } finally {
            isLoading = false; // Set loading state to false after data is fetched
        }
    });
</script>

<div class="min-h-screen flex flex-col">
    <!-- Header -->
    <header class="bg-gradient-to-r from-orange-600 to-orange-500 text-white shadow-lg">
        <div class="container mx-auto px-6 py-4 flex items-center justify-between">
          <!-- Logo and Branding -->
          <div class="flex items-center space-x-4">
            <img src="/logo.jpeg" alt="SRA BAO Logo" class="w-16 h-16 rounded-full border-4 border-white shadow-md" />
            <div>
              <h1 class="text-3xl font-extrabold leading-tight tracking-wide">SRA BAO</h1>
              <p class="text-sm font-medium opacity-90">Daily Reporting System</p>
            </div>
          </div>
      
         
        </div>
      </header>

    <!-- Main Content -->
    <main class="flex-grow bg-gradient-to-br from-orange-100 via-white to-orange-50 p-8">
        <h1 class="text-4xl font-extrabold text-orange-700 mb-8 text-center">Work Report Table</h1>

        <!-- Loading Spinner -->
        {#if isLoading}
            <div class="fixed inset-0 flex items-center justify-center z-40 bg-white bg-opacity-75">
                <div class="w-16 h-16 border-6 border-t-8 border-orange-500 border-solid rounded-full animate-spin"></div>
            </div>
        {:else}
            <div class="overflow-x-auto rounded-lg shadow-2xl">
                <table class="w-full text-left border-collapse bg-white rounded-lg">
                    <!-- Table Header -->
                    <thead class="bg-orange-600 text-white">
                        <tr>
                            <th class="px-6 py-4 text-sm font-semibold uppercase w-32 text-center">Report Date</th>
                            <th class="px-6 py-4 text-sm font-semibold uppercase w-32 text-center">Employee Name</th>
                            <th class="px-6 py-4 text-sm font-semibold uppercase w-24 text-center">Actions</th>
                        </tr>
                    </thead>
                    <!-- Table Body -->
                    <tbody>
                        {#if data.length === 0}
                            <tr>
                                <td colspan="3" class="px-6 py-4 text-center text-gray-500">
                                    No data available
                                </td>
                            </tr>
                        {:else}
                            {#each data as row, index}
                                <tr class="even:bg-orange-100 odd:bg-orange-50 transition-all duration-300">
                                    <td class="px-6 py-4 border-b text-sm text-center text-gray-700">{new Date(row.report_date).toLocaleDateString()}</td>
                                    <td class="px-6 py-4 border-b text-sm text-center text-gray-700">{row.employee_name}</td>
                                    
                                    
                                    <td class="px-6 py-4 border-b text-sm text-center">
                                        <button 
                                            class="px-4 py-2 bg-orange-500 text-white rounded hover:bg-orange-600"
                                            on:click={() => expandedRow = expandedRow === index ? null : index}>
                                            {expandedRow === index ? "Hide" : "View"}
                                        </button>
                                    </td>
                                </tr>
                                <!-- Expanded Row Details -->
                                {#if expandedRow === index}
                                    <tr class="bg-orange-50">
                                        <td colspan="3" class="px-6 py-4">
                                            <p><strong>Employee Name:</strong> {row.employee_name}</p>
                                            <p><strong>Premises:</strong> {row.premises}</p>
                                            <p><strong>Site Location:</strong> {row.site_location}</p>
                                            <p><strong>Client Name:</strong> {row.client_name}</p>
                                            <p><strong>Scope Of Work:</strong> {row.scope_of_work}</p>
                                            <p><strong>Work Details:</strong> {row.work_details}</p>
                                            <p><strong>Joint Visits:</strong> {row.joint_visits}</p>
                                            <p><strong>Support Needed:</strong> {row.support_needed}</p>
                                            <p><strong>Status Of Work:</strong> {row.status_of_work}</p>
                                            <p><strong>Priority:</strong> {row.priority_of_work}</p>
                                            <p><strong>Next Action Plan:</strong> {row.next_action_plan}</p>
                                            <p><strong>Result:</strong> {row.result}</p>
                                            <p><strong>Type Of Work:</strong> {row.type_of_work}</p>
                                            <p><strong>Closing Time:</strong> {row.closing_time}</p>
                                            <p><strong>Contact Person:</strong> {row.contact_person_name}</p>
                                            <p><strong>Customer Email ID:</strong> {row.contact_emailid}</p>
                                        </td>
                                    </tr>
                                {/if}
                            {/each}
                        {/if}
                    </tbody>
                </table>
            </div>
        {/if}
    </main>

    <!-- Footer -->
    <footer class="bg-orange-600 text-white py-4 shadow-inner">
        <div class="container mx-auto px-4 text-center text-sm">
            <p>&copy; 2024 SRA BAO. All Rights Reserved.</p>
        </div>
    </footer>
</div>
