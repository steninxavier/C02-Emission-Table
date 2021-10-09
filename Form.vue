<template>

  <div class="p-3 mx-4">
    <h2
      class="text-2xl font-bold leading-7 text-gray-900 sm:text-3xl sm:truncate mb-4"
    >
      CO<sub>2</sub> Emission Analysis
    </h2>
    <hr/>
    <div class="flex flex-wrap content-center mt-4">
      <div class="mr-2">
        <label class="block text-gray-700 text-sm font-bold mb-2">
          Start Date
        </label>
        <datepicker
          placeholder="Select Date"
          v-model="startDate"
          class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
        ></datepicker>
      </div>
      <div class="mr-2">
        <label class="block text-gray-700 text-sm font-bold mb-2">
          End Date
        </label>
        <datepicker
          placeholder="Select Date"
          v-model="endDate"
          class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
        ></datepicker>
      </div>
      <div class="mt-7">
        <button
          @click="fetchEmissions"
          class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
        >
          Submit
        </button>
      </div>
    </div>
    <div 
      class="shadow overflow-hidden border-b border-gray-200 sm:rounded-lg mt-6" 
    >
      <table class="min-w-full divide-y divide-gray-200" >
        <thead class="bg-gray-100">
          <tr>
            <th
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
            >
              Shipment ID
            </th>
            <th
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
            >
              Weight (kg)
            </th>
            <th
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
            >
              Distance (km)
            </th>
            <th
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
            >
              Pickup Time
            </th>
            <th
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
            >
              Drop Off Time
            </th>
            <th
              class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
            >
              CO2 Emission
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(shipment, index) in shipments" :class="index % 2 !== 0 ? 'bg-gray-100': ''">
            <td class="px-6 py-4 whitespace-nowrap">{{ shipment.id }}</td>
            <td class="px-6 py-4 whitespace-nowrap">
              {{ shipment.weight_kg }} Kg
            </td>
            <td class="px-6 py-4 whitespace-nowrap">
              {{ shipment.distance_km }}
            </td>
            <td class="px-6 py-4 whitespace-nowrap">
              {{ new Date(shipment.pickup_time).toLocaleString() }}
            </td>
            <td class="px-6 py-4 whitespace-nowrap">
              {{ new Date(shipment.dropoff_time).toLocaleString() }}
            </td>
            <td class="px-6 py-4 whitespace-nowrap">
              {{ shipment.co2_emission }} T
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>

import Datepicker from "vuejs-datepicker";

export default {
  components: {
    Datepicker
  },
  data() {
    return {
      startDate: new Date("2021-06-01"),
      endDate: new Date("2021-06-30"),
      shipments: []
    };
  },
  methods: {
    fetchEmissions() {
      this.$axios
        .get("/api", {
          params: {
            startDate: this.startDate.toLocaleDateString("EN-CA"),
            endDate: this.endDate.toLocaleDateString("EN-CA")
          }
        })
        .then(res => {
          this.shipments = res.data;
        });
    }
  }
};
</script>
