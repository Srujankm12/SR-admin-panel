<script>
    import Header from "$lib/header.svelte";
import { onMount } from "svelte";
    import { fade } from "svelte/transition";

    let data = [];
    let filteredData = [];
    let isLoading = true;
    let searchQuery = "";
    let isModalOpen = false;
    let employeeToDelete = null;
    let successMessage = "";
    let errorMessage = "";
    let expandedRow = null;
    let showDownloadMessage = false; 

    const apiUrl = `https://sr-backend-go.onrender.com/admin/fetch`;
    const deleteUrl = `https://sr-backend-go.onrender.com/admin/delete/`;
    const downloadexcel = `https://sr-backend-go.onrender.com/excel`;

      const downloadexceltechnical = async () => {
        try {
            const response = await fetch(downloadexcel, {
                method: "GET",                     
            });
            if (response.ok) {
                const blob = await response.blob();
                const url = window.URL.createObjectURL(blob);
                const a = document.createElement("a");
                a.href = url;
                a.download = "technical.xlsx";
                a.click();
                window.URL.revokeObjectURL(url);
            } else {
                console.error("Failed to download Excel file:", response.statusText);
            }
        } catch (error) {
            console.error("Error downloading Excel file:", error);
        }
        showDownloadMessage = true;
        setTimeout(() => {
            showDownloadMessage = false;
        }, 3000);
    };


    onMount(async () => {
        try {
            const response = await fetch(apiUrl);
            if (response.ok) {
                const jsonResponse = await response.json();
                data = Array.isArray(jsonResponse) ? jsonResponse : [jsonResponse];
                filteredData = [...data];
            } else {
                console.error("Failed to fetch data:", response.statusText);
            }
        } catch (error) {
            console.error("Error fetching data:", error);
        } finally {
            isLoading = false;
        }
    });

    function filterResults() {
        const query = searchQuery.toLowerCase();
        filteredData = data.filter((row) => {
            const formattedDate = new Date(row.report_date).toLocaleDateString("en-GB");
            return (
                row.employee_name.toLowerCase().includes(query) ||
                formattedDate.includes(query)
            );
        });
    }

    function confirmDelete(employee) {
        employeeToDelete = employee;
        isModalOpen = true;
    }

    async function deleteEmployee() {
        if (!employeeToDelete) return;
        const { emp_id, employee_name } = employeeToDelete;

        const response = await fetch(deleteUrl + emp_id, { method: "GET" });

        if (response.ok) {
            filteredData = filteredData.filter((row) => row.emp_id !== emp_id);
            successMessage = `Successfully deleted report for ${employee_name}`;
        } else {
            errorMessage = `Failed to delete report for ${employee_name}`;
        }

        isModalOpen = false;
        employeeToDelete = null;

        setTimeout(() => {
            successMessage = "";
            errorMessage = "";
        }, 4000);
    }

    function closeModal() {
        isModalOpen = false;
        employeeToDelete = null;
    }

</script>

{#if successMessage || errorMessage}
<div class="fixed bottom-14 right-5 bg-black text-white font-semibold p-4 rounded-lg shadow-2xl duration-300 flex items-center space-x-2"
     transition:fade>
    <i class={successMessage ? "fa-solid fa-circle-check text-green-500" : "fa-solid fa-circle-exclamation text-red-500"}></i>
    <span>{successMessage || errorMessage}</span>
</div>
{/if}
<div class="min-h-screen flex flex-col bg-gray-100 text-gray-900">
 <Header />
    <main class="flex-grow bg-white py-28 px-4">
        <div class="relative w-full flex justify-end bg-white">
        <input
        type="text"
        bind:value={searchQuery}
        on:input={filterResults}
        placeholder="Search by EmployeeName or Date"
        class="px-3 py-1  w-80 mb-4 shadow-md border border-gray-300 rounded-lg focus:ring-2 focus:ring-black focus:outline-none "
      />
    </div>
        {#if isLoading}
            <div class="flex justify-center items-center h-full">
                <div class="animate-spin h-12 w-12 rounded-full border-t-4 border-gray-800"></div>
            </div>
        {:else}
            <div class="overflow-x-auto rounded-lg shadow-md bg-white">
                <table class="w-full text-left border-collapse">
                    <thead class="bg-black text-white">
                        <tr>
                            <th class="px-6 py-3 text-sm font-semibold uppercase text-center">Report Date</th>
                            <th class="px-6 py-3 text-sm font-semibold uppercase text-center">Employee Name</th>
                            <th class="px-6 py-3 text-sm font-semibold uppercase text-center">Premises</th>
                            <th class="px-6 py-3 text-sm font-semibold uppercase text-center">Site Location</th>
                            <th class="px-6 py-3 text-sm font-semibold uppercase text-center">Client Name</th>
                            <th class="px-6 py-3 text-sm font-semibold uppercase text-center">Actions</th>
                        </tr>
                    </thead>
                    <tbody>
                        {#if filteredData.length === 0}
                            <tr>
                                <td colspan="6" class="px-6 py-8 text-center text-gray-500 text-lg">
                                    No data available.
                                </td>
                            </tr>
                        {:else}
                            {#each filteredData as row, index}
                                <tr class="bg-white hover:bg-gray-100 transition-all">
                                    <td class="px-6 py-4 border-b text-sm text-center">{new Date(row.report_date).toLocaleDateString("en-GB")}</td>
                                    <td class="px-6 py-4 border-b text-sm text-center">{row.employee_name}</td>
                                    <td class="px-6 py-4 border-b text-sm text-center">{row.premises}</td>
                                    <td class="px-6 py-4 border-b text-sm text-center">{row.site_location}</td>
                                    <td class="px-6 py-4 border-b text-sm text-center">{row.client_name}</td>
                                    <td class="px-6 py-4 border-b text-sm text-center">
                                        <button class="px-4 py-2 bg-black text-white rounded" on:click={() => expandedRow = expandedRow === index ? null : index}>
                                            {expandedRow === index ? "Hide" : "View"} 
                                        </button>
                                        <button class="ml-2 px-4 py-2 bg-red-600 text-white rounded" on:click={() => confirmDelete(row)}>
                                            Delete
                                        </button>
                                    </td>
                                </tr>
                                {#if expandedRow === index}
                                    <tr class="bg-white">
                                        <td colspan="3" class="px-6 py-4">
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
            <div class="py-8 flex justify-center items-center fixed bottom-3  left-0 right-0 text-center">
                <button class="text-white bg-black rounded-md px-9 py-2" 
                     on:click={downloadexceltechnical}
                >
                    Download
                </button>
            </div>
        {/if}
    </main>
    {#if showDownloadMessage}
    <div class="fixed bottom-12 right-4 bg-black text-white font-semibold p-4 rounded-md shadow-2xl duration-300" transition:fade>
        Excel downloaded successfully!
    </div>
    {/if}

    {#if isModalOpen}
    <div class="fixed inset-0 bg-black bg-opacity-50 flex justify-center items-center z-50">
        <div class="bg-white p-6 rounded-lg shadow-lg w-96 text-center">
            <h2 class="text-lg font-semibold mb-4">Confirm Deletion</h2>
            <p class="text-sm mb-4">Are you sure you want to delete the report for <strong>{employeeToDelete?.employee_name}</strong>?</p>
            <div class="flex justify-center space-x-4">
                <button class="px-4 py-2 bg-gray-300 text-black rounded" on:click={closeModal}>Cancel</button>
                <button class="px-4 py-2 bg-red-500 text-white rounded" on:click={deleteEmployee}>Delete</button>
            </div>
        </div>
    </div>
    {/if}
</div>
