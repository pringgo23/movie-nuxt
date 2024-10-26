<template>
  <div class="w-full py-2.5 bg-[#ffffff08]">
    <div class="flex mx-auto justify-center">
        <div class="flex gap-9 justify-center">
          <img src="/movie/moovietime-logo.svg" alt="logo" />
          <div class="flex items-center bg-gray-800 rounded-md px-3">
            <img src="/movie/movie-icon.svg" alt="movie-icon">
            <input
              type="text"
              placeholder="Find movie"
              class="bg-transparent text-white placeholder-gray-400 px-4 py-2 focus:outline-none w-64"
            />
            <button class="px-3 text-gray-400">
              <img src="/movie/search-icon.svg" alt="search-icon">
            </button>
          </div>
          <div class="flex gap-4 items-center text-white antialiased w-full">
          <a href="#" class="hover:text-orange-500 flex gap-2 items-center"> <span><img src="/movie/grid-icon.svg" alt="grid-icon"></span> Categories</a>
          <a href="#" class="hover:text-orange-500">Movies</a>
          <a href="#" class="hover:text-orange-500">TV Shows</a>
          <a href="#" class="hover:text-orange-500">Login</a>
        </div>
        </div>
    </div>
  </div>
</template>

<script setup lang="ts">


</script>

<style></style>
