<script>
    // Example dataset for the table
    let data = [
        {
            report_date: "2024-12-12",
            employee_name: "John Doe",
            premises: "Main Office",
            site_location: "New York",
            client_name: "Acme Corp",
            scope_of_work: "Maintenance",
            work_details: "Inspected and repaired HVAC system",
            joint_visits: "Yes",
            support_needed: "Equipment",
            status_of_work: "Completed",
            priority_of_work: "High",
            next_action_plan: "Follow-up in 3 months",
            result: "Successful",
            type_of_work: "Repair",
            closing_time: "16:00",
            contact_person_name: "Jane Smith",
            customer_emailid: "jane.smith@acme.com",
        },
        // Add more objects as needed
    ];
</script>

<div class="min-h-screen bg-gradient-to-br from-orange-100 via-white to-orange-50 p-8">
    <h1 class="text-4xl font-extrabold text-orange-700 mb-8 text-center">Work Report Table</h1>

    <div class="overflow-x-auto rounded-lg shadow-2xl">
        <table class="w-full text-left border-collapse bg-white rounded-lg">
            <!-- Table Header -->
            <thead class="bg-orange-600 text-white">
                <tr>
                    <th class="px-6 py-4 text-sm font-semibold uppercase">Report Date</th>
                    <th class="px-6 py-4 text-sm font-semibold uppercase">Employee Name</th>
                    <th class="px-6 py-4 text-sm font-semibold uppercase">Premises</th>
                    <th class="px-6 py-4 text-sm font-semibold uppercase">Site Location</th>
                    <th class="px-6 py-4 text-sm font-semibold uppercase">Client Name</th>
                    <th class="px-6 py-4 text-sm font-semibold uppercase">Scope Of Work</th>
                    <th class="px-6 py-4 text-sm font-semibold uppercase">Work Details</th>
                    <th class="px-6 py-4 text-sm font-semibold uppercase">Joint Visits</th>
                    <th class="px-6 py-4 text-sm font-semibold uppercase">Support Needed</th>
                    <th class="px-6 py-4 text-sm font-semibold uppercase">Status Of Work</th>
                    <th class="px-6 py-4 text-sm font-semibold uppercase">Priority Of Work</th>
                    <th class="px-6 py-4 text-sm font-semibold uppercase">Next Action Plan</th>
                    <th class="px-6 py-4 text-sm font-semibold uppercase">Result</th>
                    <th class="px-6 py-4 text-sm font-semibold uppercase">Type Of Work</th>
                    <th class="px-6 py-4 text-sm font-semibold uppercase">Closing Time</th>
                    <th class="px-6 py-4 text-sm font-semibold uppercase">Contact Person Name</th>
                    <th class="px-6 py-4 text-sm font-semibold uppercase">Customer Email ID</th>
                </tr>
            </thead>
            <!-- Table Body -->
            <tbody>
                {#each data as row}
                    <tr class="even:bg-orange-100 odd:bg-orange-50 hover:bg-orange-200 transition">
                        <td class="px-6 py-4 border-b text-sm text-gray-700">{row.report_date}</td>
                        <td class="px-6 py-4 border-b text-sm text-gray-700">{row.employee_name}</td>
                        <td class="px-6 py-4 border-b text-sm text-gray-700">{row.premises}</td>
                        <td class="px-6 py-4 border-b text-sm text-gray-700">{row.site_location}</td>
                        <td class="px-6 py-4 border-b text-sm text-gray-700">{row.client_name}</td>
                        <td class="px-6 py-4 border-b text-sm text-gray-700">{row.scope_of_work}</td>
                        <td class="px-6 py-4 border-b text-sm text-gray-700">{row.work_details}</td>
                        <td class="px-6 py-4 border-b text-sm text-gray-700">{row.joint_visits}</td>
                        <td class="px-6 py-4 border-b text-sm text-gray-700">{row.support_needed}</td>
                        <td class="px-6 py-4 border-b text-sm text-gray-700">{row.status_of_work}</td>
                        <td class="px-6 py-4 border-b text-sm text-gray-700">{row.priority_of_work}</td>
                        <td class="px-6 py-4 border-b text-sm text-gray-700">{row.next_action_plan}</td>
                        <td class="px-6 py-4 border-b text-sm text-gray-700">{row.result}</td>
                        <td class="px-6 py-4 border-b text-sm text-gray-700">{row.type_of_work}</td>
                        <td class="px-6 py-4 border-b text-sm text-gray-700">{row.closing_time}</td>
                        <td class="px-6 py-4 border-b text-sm text-gray-700">{row.contact_person_name}</td>
                        <td class="px-6 py-4 border-b text-sm text-gray-700">{row.customer_emailid}</td>
                    </tr>
                {/each}
            </tbody>
        </table>
    </div>
</div>
