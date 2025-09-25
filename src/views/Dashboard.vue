<template>
  <section class="space-y-6">
    <Toolbar
      :areas="areas"
      :area-id="form.areaId"
      :from="form.from"
      :to="form.to"
      @update:areaId="form.areaId = $event"
      @update:from="form.from = $event"
      @update:to="form.to = $event"
      @view="updateData"
    />
    <ChartComponent :chart-data="chartData" />
    <TableComponent :areas="table.areas" :rows="table.rows" />
  </section>
</template>

<script setup>
import { reactive, ref, onMounted, computed } from "vue";
import axios from "axios";
import Toolbar from "@/components/Toolbar.vue";
import ChartComponent from "@/components/ChartComponent.vue";
import TableComponent from "@/components/TableComponent.vue";

const API = import.meta.env.VITE_API;

const areas = ref([]);
const form = reactive({ areaId: 0, from: "", to: "" });
const chartData = ref([]);
const table = reactive({ areas: [], rows: [] });

const filteredAreas = computed(() => {
  if (!form.areaId || form.areaId === 0) return areas.value; // All areas jika 0
  return areas.value.filter((area) => area.area_id === form.areaId);
});

async function loadAreas() {
  const { data } = await axios.get(`${API}/areas`);
  areas.value = data;

  const today = new Date();
  const to = today.toISOString().slice(0, 10);
  const from = new Date(today.getTime() - 29 * 86400000)
    .toISOString()
    .slice(0, 10);
  form.from = from;
  form.to = to;
}

async function loadChart() {
  const { data } = await axios.get(`${API}/charts`, {
    params: {
      area_id: form.areaId,
      from: form.from || "",
      to: form.to || "",
    },
  });
  chartData.value = data || [];
}

async function loadTable() {
  try {
    const { data } = await axios.get(`${API}/tables`, {
      params: {
        area_id: form.areaId || 0,
        from: form.from || "",
        to: form.to || "",
      },
    });
    table.areas =
      data.areas ||
      (form.areaId && form.areaId !== 0 ? filteredAreas.value : areas.value) ||
      [];
    table.rows = data.rows || [];
  } catch (error) {
    table.areas =
      form.areaId && form.areaId !== 0 ? filteredAreas.value : areas.value;
    table.rows = [];
  }
}

async function updateData() {
  await loadChart();
  await loadTable();
}

onMounted(async () => {
  await loadAreas();
  await loadChart();
  await loadTable();
});
</script>
