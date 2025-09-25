<template>
  <div class="bg-white border border-slate-200 rounded-2xl p-5 shadow-sm">
    <h3 class="text-sm font-semibold text-slate-700 mb-3">
      Ringkasan Brand × Area
    </h3>
    <div class="overflow-x-auto">
      <table class="min-w-full text-sm border border-slate-200 rounded-lg mb-4">
        <thead>
          <tr class="bg-slate-50 text-slate-600 border border-slate-200">
            <th class="px-3 py-2 text-left font-medium">Brand</th>
            <th
              v-for="a in areas"
              :key="a.area_id"
              class="px-3 py-2 font-medium border border-slate-200"
            >
              {{ a.area_name }}
            </th>
          </tr>
        </thead>
        <tbody class="divide-y divide-slate-100">
          <tr v-for="(r, idx) in rows" :key="idx" class="hover:bg-slate-50">
            <td
              class="px-3 py-2 font-medium text-slate-800 border border-slate-200"
            >
              {{ r["Brand"] }}
            </td>
            <td
              v-for="a in areas"
              :key="a.area_id"
              class="px-3 py-2 text-center border border-slate-200"
            >
              <span
                v-if="r[a.area_name] != null"
                class="inline-flex items-center justify-center rounded-lg bg-blue-50 text-blue-700 px-2 py-0.5 font-medium"
              >
                {{ r[a.area_name] }}%
              </span>
              <span v-else class="text-slate-400">–</span>
            </td>
          </tr>
        </tbody>
      </table>
      <p
        v-if="!rows.length"
        class="text-slate-500 text-sm mt-2 p-6 text-center"
      >
        Belum ada data.
      </p>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  areas: { type: Array, required: true },
  rows: { type: Array, default: () => [] },
});
</script>
