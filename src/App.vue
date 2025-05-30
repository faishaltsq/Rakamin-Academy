<script setup>
import EcommerceDashboard from './components/dashboard.vue'
</script>

<template>
  <EcommerceDashboard />
</template>

<style>
body, html {
  margin: 0;
  padding: 0;
  width: 100%;
  min-height: 100vh;
  background-color: white !important;
  color: black !important;
}

/* Override any potential theme colors to ensure black text */
* {
  color-scheme: light !important;
}

/* Force black text on important elements */
h1, h2, h3, h4, h5, h6, p, span, div, a, button {
  color: inherit;
}
</style>
