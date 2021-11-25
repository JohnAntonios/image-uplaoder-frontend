<template>
  <footer>
    <a href="//github.com/JohnAntonios" target="_blank">© John Antonios {{ currentYear }}</a>
  </footer>
</template>

<script lang="ts" setup>
import { computed } from "@vue/reactivity";

const currentYear = computed(() => new Date().getFullYear());
</script>

<style scoped>
footer {
  margin-top: auto;
  margin-bottom: 10px;
}

a {
  text-align: center;
  color: #2f80ed;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}
</style>