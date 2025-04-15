<script setup lang="ts">
import { ref, onMounted, computed } from 'vue'
import ReportFilters from './components/ReportFilters.vue'

// Report types for filter dropdown
const reportTypes = ['Financial', 'Operational', 'Marketing', 'HR', 'All']
const selectedType = ref('All')

// Date range for filtering
const startDate = ref('')
const endDate = ref('')

// Search functionality
const searchQuery = ref('')

const allReports = ref([
  { id: 1, name: 'Q1 Financial Summary', type: 'Financial', date: '2025-03-31', size: '2.4 MB' },
  {
    id: 2,
    name: 'March Operations Review',
    type: 'Operational',
    date: '2025-03-15',
    size: '1.8 MB',
  },
  {
    id: 3,
    name: 'Q1 Marketing Performance',
    type: 'Marketing',
    date: '2025-03-31',
    size: '3.5 MB',
  },
  { id: 4, name: 'Employee Satisfaction Survey', type: 'HR', date: '2025-02-15', size: '1.2 MB' },
  {
    id: 5,
    name: 'February Financial Statement',
    type: 'Financial',
    date: '2025-02-28',
    size: '1.9 MB',
  },
])

// Filtered reports based on selected type, date range, and search
const filteredReports = computed(() => {
  return allReports.value.filter((report) => {
    // Filter by type
    if (selectedType.value !== 'All' && report.type !== selectedType.value) return false

    // Filter by date range
    if (startDate.value && report.date < startDate.value) return false
    if (endDate.value && report.date > endDate.value) return false

    // Filter by search query
    if (searchQuery.value && !report.name.toLowerCase().includes(searchQuery.value.toLowerCase()))
      return false

    return true
  })
})

// Download function (placeholder)
const downloadReport = (reportId: number) => {
  // In a real app, this would initiate a download
  console.log(`Downloading report with ID: ${reportId}`)
  alert(`Download started for report ID: ${reportId}`)
}

// Load reports (would be an API call in a real app)
onMounted(() => {
  console.log('Component mounted, reports loaded')
})
</script>

<template>
  <header>
    <div class="header-content">
      <h1>Organization Reports Portal</h1>
      <div class="user-info">Welcome, Admin</div>
    </div>
  </header>

  <main>
    <!-- Use the new ReportFilters component with v-model for two-way binding -->
    <ReportFilters
      :report-types="reportTypes"
      v-model:selected-type="selectedType"
      v-model:start-date="startDate"
      v-model:end-date="endDate"
      v-model:search-query="searchQuery"
    />

    <div class="reports-container">
      <h2>
        Available Reports <span v-if="filteredReports.length">({{ filteredReports.length }})</span>
      </h2>

      <div v-if="filteredReports.length === 0" class="no-reports">
        No reports match your criteria. Try adjusting your filters.
      </div>

      <table v-else class="reports-table">
        <thead>
          <tr>
            <th>Report Name</th>
            <th>Type</th>
            <th>Date</th>
            <th>Size</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="report in filteredReports" :key="report.id">
            <td>{{ report.name }}</td>
            <td>{{ report.type }}</td>
            <td>{{ new Date(report.date).toLocaleDateString() }}</td>
            <td>{{ report.size }}</td>
            <td>
              <button @click="downloadReport(report.id)" class="download-btn">Download</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </main>
</template>

<style scoped>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

body {
  background-color: #1a1a1a;
  color: #e0e0e0;
}

header {
  background-color: #121212;
  color: white;
  padding: 1.25rem 2rem;
  box-shadow: 0 3px 10px rgba(0, 0, 0, 0.3);
}

.header-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  max-width: 1200px;
  margin: 0 auto;
}

.header-content h1 {
  font-size: 1.6rem;
  font-weight: 600;
  color: #ffffff;
}

.user-info {
  background-color: rgba(255, 255, 255, 0.1);
  padding: 0.5rem 1rem;
  border-radius: 20px;
  font-size: 0.9rem;
}

main {
  min-width: 75vw;
  margin: 2rem auto;
  padding: 0 1.5rem;
}

.reports-container {
  background-color: #242424;
  border-radius: 10px;
  padding: 1.5rem;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
}

h2 {
  margin-bottom: 1.5rem;
  color: #ffffff;
  font-weight: 600;
}

h2 span {
  font-size: 0.9rem;
  background-color: #333333;
  padding: 0.25rem 0.6rem;
  border-radius: 20px;
  color: #aaaaaa;
  margin-left: 0.5rem;
  font-weight: 500;
}

.reports-table {
  width: 100%;
  border-collapse: separate;
  border-spacing: 0;
}

.reports-table th,
.reports-table td {
  padding: 1rem;
  text-align: left;
  border-bottom: 1px solid #333333;
}

.reports-table th {
  background-color: #1a1a1a;
  font-weight: 600;
  color: #aaaaaa;
  font-size: 0.9rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.reports-table tr:hover {
  background-color: #2a2a2a;
}

.reports-table tr:last-child td {
  border-bottom: none;
}

.download-btn {
  background-color: #10b981;
  color: white;
  border: none;
  padding: 0.6rem 1.2rem;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 500;
  font-size: 0.9rem;
  transition: all 0.2s;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
}

.download-btn:hover {
  background-color: #059669;
  transform: translateY(-1px);
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.3);
}

.download-btn:active {
  transform: translateY(0);
  box-shadow: none;
}

.no-reports {
  padding: 3rem 1rem;
  text-align: center;
  color: #aaaaaa;
  background-color: #1a1a1a;
  border-radius: 8px;
  font-weight: 500;
}

@media (max-width: 768px) {
  .reports-table th,
  .reports-table td {
    padding: 0.75rem 0.5rem;
    font-size: 0.9rem;
  }
}
</style>
