<template>
  <div
    class="flex items-center min-w-[541px] text-white rounded-lg overflow-hidden"
  >
    <img
      :src="`https://image.tmdb.org/t/p/w220_and_h330_face/${dataMovie.poster_path}`"
      alt="Space Sweepers"
      class="w-[243.08px] h-[364.61px] object-cover"
    />
    <div class="p-6 w-2/3 bg-black shadow-lg h-fit">
      <div class="flex items-center">
        <span class="text-yellow-400 text-lg font-bold mr-2">★ {{ dataMovie.vote_average }}</span>
        <span class="text-lg font-semibold">{{ dataMovie.title }}</span>
      </div>
      <div class="text-gray-400 text-sm mt-1">{{ formatYear }} • Sci-Fi</div>
      <p class="mt-4 text-gray-300 line-clamp-5">
        {{ dataMovie.overview }}
      </p>
    </div>
  </div>
</template>

<script setup lang="ts">
//@ts-ignore
const props = defineProps<{
    dataMovie: Object
}>()

const formatYear = computed(() => {
    const year = props.dataMovie.release_date.split("-")[0]    
    return year
})

</script>

<style></style>
