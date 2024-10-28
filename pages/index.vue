<template>
  <div class="pb-8">
    <div v-if="status === 'success'">
      <section class="w-full">
        <div
          id="slider"
          class="flex items-center gap-x-8 mt-8 overflow-x-scroll w-full"
        >
          <div v-for="movie in movies?.results" :key="movie">
            <CardSlider
              :dataMovie="movie"
              :key="'slider3-' + movie"
              :style="{
                transform: `translateX(${-10 * (currentSlide - 1) * 10}px)`,
              }"
            />
          </div>
        </div>
        <div class="flex justify-center items-center">
          <div id="dots-slider">
            <div class="flex justify-center mt-4">
              <button
                v-for="(movie, index) in movies?.results"
                :key="index"
                :class="{
                  'bg-orange-500 w-10': currentSlide === index,
                  'bg-gray-400 w-3': currentSlide !== index,
                }"
                class="h-3 rounded-full mx-1 transition-colors duration-200"
                @click="setCurrentSlide(index)"
              ></button>
            </div>
          </div>
        </div>
      </section>
      <div class="relative">
        <div class="h-[333px] w-full bg-[#ffffff07] z-0"></div>

        <section class="-mt-52 z-1 flex justify-center items-center px-40">
          <div>
            <div class="flex justify-between">
              <div>
                <div class="h-2 w-32 bg-[#E74C3C]"></div>
                <p class="antialiased text-2xl font-semibold mt-3">
                  Discover Movies
                </p>
              </div>

              <div class="flex gap-2 items-center">
                <p>my movies</p>
                <div
                  class="flex items-center justify-center px-4 py-2 bg-gray-800 rounded-full text-white font-medium space-x-2"
                >
                  <span class="text-[12px] font-semibold">2</span>
                  <span class="text-[13px] antialiased">movies</span>
                </div>
              </div>
            </div>
            <div class="flex gap-8 mt-14">
              <!-- --- component filter --- -->
              <Sorting
                v-model:sortBy="sortBy"
                v-model:selectedGenres="selectedGenres"
                :genres="genres"
              ></Sorting>
              <div class="grid grid-cols-4 gap-2">
                <div
                  v-for="item in productsFetched"
                  :key="item.id"
                  class="pb-3"
                >
                  <BaseImageThumbnail :item="item"></BaseImageThumbnail>
                  <p class="font-semibold pr-4 pt-1">{{ item.title }}</p>
                  <p class="antialiased text-[14px] text-[#929292]">{{ item.release_date.split("-")[0] }}</p>
                </div>
              </div>
            </div>
          </div>
        </section>
      </div>
      <div class="flex justify-center items-center">
          <button class="px-8 py-1 bg-[#FF0000] rounded-full text-white font-medium" @click="page++">Load More</button>
      </div>
    </div>
    <div v-else-if="status === 'error'" class="">Error</div>
    <div v-else class="">Loading...</div>
  </div>
</template>

<script setup lang="ts">
const id = ref(null);
const selectedGenres = ref([]);
const page = ref(1);
const productsFetched: any = ref([]);
const sortBy = ref("popularity.desc");
const selectedGenresString = computed(() => selectedGenres.value.join("%2C"));

const headers = {
  Authorization:
    "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJiMmYwZWU5MjE3ZDgyOWI3ZGVhYTNiYTI5YTZmODEzYyIsIm5iZiI6MTcyOTk1NzE0My4wNTkyMzUsInN1YiI6IjYyNjI1MmVjMTY4ZWEzMTU1N2QzMzBkMSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.UVmWHMpoqD0qCekycgjftOuIQHmf6yzZceERpwWlvRs",
  Accept: "application/json",
};

const url = computed(() => {
  return `https://api.themoviedb.org/3/discover/movie?include_adult=false&include_video=false&language=en-US&page=${page.value}&sort_by=${sortBy.value}&with_genres=${selectedGenresString.value}`;
});

const { data, status } = await useFetch(url, {
  // @ts-ignore
  headers,
  lazy: true,
  server: false,
  watch: [selectedGenres, sortBy, page],
  transform: (res: any) => {
    if (page.value === 1) {
      productsFetched.value = res.results;
    } else {
      productsFetched.value = [...productsFetched.value, ...res.results];
    }
    return productsFetched.value;
  },
});

const { data: genres } = await useFetch(
  "https://api.themoviedb.org/3/genre/movie/list?language=en",
  {
    // @ts-ignore
    headers,
    lazy: true,
    server: false,
  }
);

const { data: movies } = await useFetch(
  "https://api.themoviedb.org/3/trending/movie/day?language=en-US",
  {
    // @ts-ignore
    headers,
    lazy: true,
    server: false,
  }
);


const currentSlide = ref(0);

const setCurrentSlide = (index: number) => {
  currentSlide.value = index;
};

</script>

<style scoped>
#slider {
  scrollbar-width: none;
  -ms-overflow-style: none;
}

/* chrome */
#slider::-webkit-scrollbar {
  width: 0px;
  background: transparent;
}
</style>
