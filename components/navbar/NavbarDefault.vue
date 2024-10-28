<template>
  <div class="w-full py-2.5 bg-[#ffffff08]">
    <div class="flex mx-auto justify-center">
      <div class="flex gap-9 justify-center">
        <img src="/movie/moovietime-logo.svg" alt="logo" />
        <div class="flex items-center bg-gray-800 rounded-md px-3">
          <img src="/movie/movie-icon.svg" alt="movie-icon" />
          <input
            type="text"
            v-model="search"
            placeholder="Find movie"
            class="bg-transparent text-white placeholder-gray-400 px-4 py-2 focus:outline-none w-64"
          />
          <div>
            <ul>
              <li v-for="item in searchResult" :key="item.id">
                {{ item.title }}
              </li>
            </ul>
          </div>
          <button class="px-3 text-gray-400">
            <img src="/movie/search-icon.svg" alt="search-icon" />
          </button>
        </div>
        <div class="flex gap-4 items-center text-white antialiased w-full">
          <a href="#" class="hover:text-orange-500 flex gap-2 items-center">
            <span><img src="/movie/grid-icon.svg" alt="grid-icon" /></span>
            Categories</a
          >
          <a href="#" class="hover:text-orange-500">Movies</a>
          <a href="#" class="hover:text-orange-500">TV Shows</a>
          <a href="#" class="hover:text-orange-500">Login</a>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
const search = ref(null);
const debouncedSearch = ref("");

const headers = {
  Authorization:
    "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI1ZTQxMWE4YjVhYzI3M2RhNDI0ODZhZGRiMDQ1YjY4OSIsIm5iZiI6MTcyOTk0OTE2MS43ODc2MzYsInN1YiI6IjVmNzcyMGU4MjE2MjFiMDAzNTNhMDJjYyIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.85FcQ1-dap9nr6dZH5ufF8Q4mLRBIRQZrE0GVw0nWmA",
  Accept: "application/json",
};

function debounce(fn: Function, delay: number) {
  // @ts-ignore
  let timeoutId: NodeJS.Timeout;
  return function (...args: any[]) {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => fn(...args), delay);
  };
}

watch(
  search,
  debounce((newValue: string) => {
    debouncedSearch.value = newValue;
  }, 1000)
);

const searchUrl = computed(() => {
  return `https://api.themoviedb.org/3/search/movie?query=${search.value}&include_adult=false&language=en-US&page=1`;
});

const { data: searchResult } = await useFetch(searchUrl, {
  // @ts-ignore
  headers,
  lazy: true,
  server: false,
  watch: [debouncedSearch],
});
</script>

<style></style>
