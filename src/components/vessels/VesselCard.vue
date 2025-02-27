<script setup>
import { useVesselsStore } from "../../../stores/vessels";

const props = defineProps({
  vessel: {
    type: Object,
    required: true,
    default: () => ({})
  }
});

const vesselsStore = useVesselsStore();

// Function to toggle status
const toggleStatus = (type) => {
  vesselsStore.updateVesselStatus(props.vessel.id, type);
};
</script>

<template>
  <div 
    v-if="props.vessel && props.vessel.vesselName" 
    class="bg-dark text-white p-6 rounded-lg shadow-xl w-full max-w-sm transition-transform transform hover:scale-105"
  >
  <img src="../../assets/logo-white.png" width="30" alt="white logo" class="pb-8 mx-auto">
    <!-- Vessel Name -->
    <div class="flex flex-col items-center gap-x-2 mb-4">
      <span class="material-icons text-accent text-sm">directions_boat</span>
      <h2 class="text-lg font-bold text-accent">{{ vessel.vesselName }}</h2>
    </div>

    <!-- Product States (Click to Toggle) -->
    <div class="space-y-3">
      <!-- Mail Product State -->
      <div class="flex flex-col items-center justify-center gap-x-2 cursor-pointer" @click="toggleStatus('mailProductState')">
        <span class="material-icons text-primary text-sm">email</span>
        <p class="text-sm">
          Mail Product State:
        
        </p>
        <span
        class="text-sm"
        :class="{
          'text-green-500 font-semibold': props.vessel.mailProductState === 'Live',
          'text-red-500 font-semibold': props.vessel.mailProductState === 'Disabled'
        }"
      >
        {{ props.vessel.mailProductState }}
      </span>
      </div>

      <!-- Cyber Product State -->
      <div class="flex flex-col items-center gap-x-2 cursor-pointer" @click="toggleStatus('cyberProductState')">
        <span class="material-icons text-accent text-sm">security</span>
        <p class="text-sm">
          Cyber Product State:
      
        </p>
        <span
        class="text-sm"
        :class="{
          'text-green-500 font-semibold': props.vessel.cyberProductState === 'Live',
          'text-red-500 font-semibold': props.vessel.cyberProductState === 'Disabled'
        }"
      >
        {{ props.vessel.cyberProductState }}
      </span>
      </div>
    </div>

    <!-- Asset Count -->
    <div class="mt-4 flex items-center space-x-2 bg-navy text-white py-2 px-3 rounded-lg">
      <span class="material-icons text-white text-sm">storage</span>
      <p class="text-sm font-semibold">Assets: {{ props.vessel.assetCount }}</p>
    </div>
  </div>

  <!-- Fallback if vessel is not yet loaded -->
  <div v-else class="text-gray-500">Loading...</div>
</template>

<style scoped></style>
