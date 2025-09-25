<template>
  <div class="min-h-screen bg-white text-slate-800 font-sans">
    <header class="border-b border-slate-200 bg-white shadow-sm">
      <div
        class="container mx-auto flex items-center justify-between py-4 px-4"
      >
        <h1 class="text-2xl font-bold tracking-tight">
          <span class="text-blue-600">Dashboard</span>
        </h1>
      </div>
    </header>

    <main class="container mx-auto py-6 px-4">
      <Dashboard />
    </main>

    <footer class="border-t border-slate-200">
      <div class="container mx-auto py-4 text-center text-xs text-slate-500">
        © 2025 — Yusuf Bahtiar
      </div>
    </footer>
  </div>
</template>

<script setup>
import Dashboard from "./views/Dashboard.vue";
</script>
