<template>
  <div class="bg-white border border-slate-200 rounded-2xl p-5 shadow-sm">
    <div class="flex flex-col md:flex-row md:items-end gap-4">
      <div class="flex-1">
        <label
          class="text-s font-medium text-slate-600 mb-1 flex items-center gap-1"
        >
          <MapPin class="w-4 h-4 text-blue-600" /> Area
        </label>
        <select
          :value="areaId"
          class="w-full rounded border-slate-300 focus:border-blue-600 p-1 border-2 focus:ring focus:ring-blue-600/30"
          @change="emit('update:areaId', $event.target.value)"
        >
          <option v-for="a in areas" :key="a.area_id" :value="a.area_id">
            {{ a.area_name }}
          </option>
        </select>
      </div>

      <div>
        <label
          class="text-s font-medium text-slate-600 mb-1 flex items-center gap-1"
        >
          <Calendar class="w-4 h-4 text-blue-600" /> Date From
        </label>
        <input
          type="date"
          :value="from"
          class="rounded border-slate-300 focus:border-blue-600 w-full p-1 border-2 focus:ring focus:ring-blue-600/30"
          @change="emit('update:from', $event.target.value)"
        />
      </div>

      <div>
        <label
          class="text-s font-medium text-slate-600 mb-1 flex items-center gap-1"
        >
          <Calendar class="w-4 h-4 text-blue-600" /> Date To
        </label>
        <input
          type="date"
          :value="to"
          class="rounded border-slate-300 focus:border-blue-600 w-full p-1 border-2 focus:ring focus:ring-blue-600/30"
          @change="emit('update:to', $event.target.value)"
        />
      </div>

      <div class="md:ml-auto">
        <button
          @click="emit('view')"
          class="flex items-center justify-center gap-2 rounded-xl bg-blue-600 w-full px-7 py-1.5 text-white font-medium shadow hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-600/40"
        >
          <Search class="w-4 h-4" /> View
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { MapPin, Calendar, Search } from "lucide-vue-next";

defineProps({
  areas: { type: Array, required: true },
  areaId: { type: [Number, String], default: 0 },
  from: { type: String, default: "" },
  to: { type: String, default: "" },
});

const emit = defineEmits(["update:areaId", "update:from", "update:to", "view"]);
</script>
