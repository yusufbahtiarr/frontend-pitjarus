<template>
  <div class="bg-white border border-slate-200 rounded-2xl p-5 shadow-sm">
    <h3 class="text-sm font-semibold text-slate-700 mb-3">
      Persentase Nilai per Area
    </h3>
    <div class="relative h-48">
      <canvas v-if="hasChartData" ref="barRef" class="w-full h-full"></canvas>
      <div
        v-else
        class="flex items-center justify-center h-full text-slate-500"
      >
        No Chart
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, nextTick, watch } from "vue";
import Chart from "chart.js/auto";
import ChartDataLabels from "chartjs-plugin-datalabels";

Chart.register(ChartDataLabels);

const props = defineProps({
  chartData: { type: Array, default: () => [] },
});

const barRef = ref(null);
let barChart = null;
const hasChartData = ref(false);

function renderBar(labels, values) {
  if (!barRef.value) return;
  if (barChart) {
    barChart.destroy();
    barChart = null;
  }
  barChart = new Chart(barRef.value.getContext("2d"), {
    type: "bar",
    data: {
      labels,
      datasets: [
        {
          label: "Nilai",
          data: values,
          backgroundColor: "rgb(37,99,235)",
          borderColor: "rgb(37,99,235)",
          borderWidth: 1,
          borderRadius: 4,
          barThickness: 35,
        },
      ],
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        legend: { display: false },
        tooltip: { callbacks: { label: (c) => `${c.parsed.y}` } },
        datalabels: {
          anchor: "end",
          align: "end",
          offset: -2,
          color: "#111827",
          font: { weight: "bold", size: 12 },
          formatter: (v) => `${v}`,
        },
      },
      scales: {
        y: { beginAtZero: true, max: 100, ticks: { callback: (v) => v } },
      },
    },
  });
}

watch(
  () => props.chartData,
  (newData) => {
    hasChartData.value =
      newData && Array.isArray(newData) && newData.length > 0;
    if (hasChartData.value) {
      nextTick(() => {
        renderBar(
          newData.map((i) => i.area_name || "No Name"),
          newData.map((i) => i.pct || 0)
        );
      });
    } else if (barChart) {
      barChart.destroy();
      barChart = null;
    }
  },
  { immediate: true }
);
</script>
