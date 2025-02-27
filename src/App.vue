<script setup>
import { ref, computed, onMounted } from "vue";
import { useVesselsStore } from "../stores/vessels";
import { useComputersStore } from "../stores/computers";
import VesselCard from "./components/vessels/VesselCard.vue";

const vesselsStore = useVesselsStore();
const computersStore = useComputersStore();

const filterState = ref("all");
const selectedComputer = ref("all");

onMounted(() => {
  vesselsStore.fetchVessels();
  computersStore.fetchComputers();
});

const filteredVessels = computed(() => {
  let vessels = vesselsStore.vessels;

  if (filterState.value !== "all") {
    vessels = vessels.filter(
      (vessel) =>
        vessel.mailProductState === filterState.value ||
        vessel.cyberProductState === filterState.value
    );
  }

  if (selectedComputer.value !== "all") {
    vessels = vessels.filter((vessel) =>
      computersStore.computers.some(
        (computer) =>
          computer.vesselId === vessel.id &&
          computer.computerName === selectedComputer.value
      )
    );
  }

  return vessels;
});

const uniqueComputers = computed(() => {
  return ["all", ...new Set(computersStore.computers.map((c) => c.computerName))];
});
</script>

<template>
  <div class="min-h-screen bg-gray-900 text-white w-full">
    <!-- Page Title -->
    <header class="text-center py-6 p-4 bg-dark text-accent flex flex-col justify-center items-center gap-2">
      <h1 class="text-lg font-bold">MGMaritime Vessel Status</h1>
      <img src="./assets/logo.png" width="60" height="50" alt="MGMaritime Logo" />
    </header>

    <div class="flex flex-col lg:flex-row p-6">
      <!-- Sidebar Filters (Full-width on Mobile, Left Sidebar on Desktop) -->
      <aside class="w-full lg:w-1/4 bg-gray-800 p-4 rounded-lg shadow-lg mb-6 lg:mb-0">
        <h2 class="text-xl font-semibold mb-4 text-accent">Filters</h2>

        <!-- State Filter -->
        <div class="mb-4">
          <label for="filter" class="block text-lg">Filter by State:</label>
          <p class="text-bold text-xs">(click to toggle state)</p>
          <select
            id="filter"
            v-model="filterState"
            class="w-full p-2 mt-2 rounded bg-dark text-white border border-gray-600"
          >
            <option value="all">All</option>
            <option value="Live">Live</option>
            <option value="Disabled">Disabled</option>
          </select>
        </div>

        <!-- Computer Filter -->
        <div>
          <label for="computerFilter" class="block text-lg">Filter by Computer:</label>
          <select
            id="computerFilter"
            v-model="selectedComputer"
            class="w-full p-2 mt-2 rounded bg-dark text-white border border-gray-600"
          >
            <option v-for="computer in uniqueComputers" :key="computer" :value="computer">
              {{ computer }}
            </option>
          </select>
        </div>
      </aside>

      <!-- Vessel Grid -->
      <main class="w-full p-4">
        <div class="grid gap-6 sm:grid-cols-1 md:grid-cols-2 lg:grid-cols-4">
          <VesselCard v-for="vessel in filteredVessels" :key="vessel.id" :vessel="vessel" />
        </div>

        <!-- No Results Message -->
        <p v-if="filteredVessels.length === 0" class="text-gray-400 mt-4 text-center">
          No vessels found.
        </p>
      </main>
    </div>
  </div>
</template>

<style scoped></style>
