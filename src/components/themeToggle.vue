<template>
  <q-toggle v-model="isDark" @update:model-value="toggleTheme" />
</template>

<script setup>
import { ref } from "vue";
import { useQuasar } from "quasar";

const $q = useQuasar();
const isDark = ref($q.dark.isActive);

const toggleTheme = () => {
  $q.dark.toggle();
};
</script>
