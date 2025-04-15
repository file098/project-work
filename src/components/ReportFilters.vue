<template>
  <div class="filters">
    <div class="filters-container">
      <div class="filter-row">
        <div class="filter-group">
          <label for="report-type">Report Type:</label>
          <select id="report-type" :value="selectedType" @change="updateSelectedType">
            <option v-for="type in reportTypes" :key="type" :value="type">{{ type }}</option>
          </select>
        </div>

        <div class="filter-group">
          <label for="start-date">From:</label>
          <input type="date" id="start-date" :value="startDate" @input="updateStartDate" />
        </div>

        <div class="filter-group">
          <label for="end-date">To:</label>
          <input type="date" id="end-date" :value="endDate" @input="updateEndDate" />
        </div>
      </div>

      <div class="filter-group search">
        <label for="search">Search:</label>
        <input
          id="search"
          type="text"
          placeholder="Search reports..."
          :value="searchQuery"
          @input="updateSearchQuery"
        />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { defineProps, defineEmits } from 'vue'

// Define props to receive data from parent
const props = defineProps({
  reportTypes: {
    type: Array as () => string[],
    required: true,
  },
  selectedType: {
    type: String,
    required: true,
  },
  startDate: {
    type: String,
    required: true,
  },
  endDate: {
    type: String,
    required: true,
  },
  searchQuery: {
    type: String,
    required: true,
  },
})

// Define emits to send data back to parent
const emit = defineEmits([
  'update:selectedType',
  'update:startDate',
  'update:endDate',
  'update:searchQuery',
])

// Event handlers
const updateSelectedType = (event: Event) => {
  emit('update:selectedType', (event.target as HTMLSelectElement).value)
}

const updateStartDate = (event: Event) => {
  emit('update:startDate', (event.target as HTMLInputElement).value)
}

const updateEndDate = (event: Event) => {
  emit('update:endDate', (event.target as HTMLInputElement).value)
}

const updateSearchQuery = (event: Event) => {
  emit('update:searchQuery', (event.target as HTMLInputElement).value)
}
</script>

<style scoped>
.filters {
  background-color: #242424;
  border-radius: 10px;
  padding: 1.5rem;
  margin-bottom: 2rem;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
}

.filters-container {
  display: flex;
  flex-direction: column;
  gap: 1.25rem;
}

.filter-row {
  display: flex;
  gap: 1.25rem;
}

.filter-group {
  display: flex;
  flex-direction: column;
  min-width: 150px;
  flex: 1;
}

.filter-group.search {
  width: 100%;
}

label {
  margin-bottom: 0.5rem;
  font-weight: 600;
  font-size: 0.9rem;
  color: #bbbbbb;
}

input,
select {
  padding: 0.7rem;
  border: 1px solid #444444;
  border-radius: 6px;
  font-size: 0.95rem;
  background-color: #333333;
  color: #e0e0e0;
  transition:
    border-color 0.2s,
    box-shadow 0.2s;
  width: 100%;
}

select {
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
  background-image: url("data:image/svg+xml;utf8,<svg fill='%23bbbbbb' height='24' viewBox='0 0 24 24' width='24' xmlns='http://www.w3.org/2000/svg'><path d='M7 10l5 5 5-5z'/><path d='M0 0h24v24H0z' fill='none'/></svg>");
  background-repeat: no-repeat;
  background-position: right 0.7rem top 50%;
  background-size: 1.2rem;
  padding-right: 2rem;
}

input:focus,
select:focus {
  outline: none;
  border-color: #666666;
  box-shadow: 0 0 0 3px rgba(100, 100, 100, 0.2);
}

input::placeholder {
  color: #777777;
}

@media (max-width: 768px) {
  .filter-row {
    flex-direction: column;
    gap: 1rem;
  }

  .filter-group {
    width: 100%;
  }
}
</style>
