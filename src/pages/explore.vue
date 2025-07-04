<template>
  <q-page class="explore">

    <!-- Search -->
    <div class="search q-mt-md">
      <q-input outline standout label="Search">
        <template v-slot:prepend>
          <q-icon name="search" />
        </template>
      </q-input>
    </div>

    <!-- Grids -->
    <div class="grid-container q-mt-md">
      <div v-for="(rowSeeds, rowIdx) in rowedSeeds" :key="rowIdx" class="grid">
        <div v-for="(seed, colIdx) in rowSeeds" :key="colIdx" class="grid-item">
          <q-img ratio="1" class="fit" :src="`https://picsum.photos/500/300?random=${seed}`" />
        </div>
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { ref, computed } from 'vue';

// generate 25 random seeds
const flatSeeds = ref(
  Array.from({ length: 25 }, () =>
    Math.floor(Math.random() * 1000) + 1
  )
);

// chunk into rows of 5
const rowedSeeds = computed(() => {
  const rows = [];
  for (let i = 0; i < flatSeeds.value.length; i += 5) {
    rows.push(flatSeeds.value.slice(i, i + 5));
  }
  return rows;
});
</script>
