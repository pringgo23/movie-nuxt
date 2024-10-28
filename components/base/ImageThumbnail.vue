<template>
  <div
    class="relative w-full bg-gray-800 rounded-lg overflow-hidden shadow-lg"
  >
    <img class="object-cover"  :src="`https://image.tmdb.org/t/p/w220_and_h330_face/${item.poster_path}`" alt="poster">

    <div
      class="absolute top-2 right-2 bg-gray-900 bg-opacity-80 text-white text-sm font-semibold px-2 py-1 rounded"
    >
      {{ item.vote_average.toFixed(1) }}
    </div>
  </div>
</template>

<script setup lang="ts">
//@ts-ignore
const props = defineProps<{
  item: Object;
}>();
</script>

<style></style>
