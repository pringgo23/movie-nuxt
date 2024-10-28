<template>
  <div>
    <section>
      <h1 class="text-secondary text-3xl font-bold underline">{{ title }}</h1>
    </section>

    <section>
      <div>
        <!-- {{ id }} -->
        <!-- <br /> -->
        <!-- <input v-model="id" /> -->
        <!-- {{ selectedGenres }}
        {{ selectedGenresString }} -->

        <div v-if="status === 'success'" class="flex gap-3">
          <!-- --- component filter --- -->
          <Sorting
            v-model:sortBy="sortBy"
            v-model:selectedGenres="selectedGenres"
            :genres="genres"
          ></Sorting>
          <div class="grid grid-cols-4">
            <div
              v-for="item in data.results"
              :key="item.id"
              class="text-center"
            >
              <img
                :src="`https://image.tmdb.org/t/p/w220_and_h330_face/${item.poster_path}`"
                alt="poster"
              />
              {{ item.title }}
            </div>
          </div>
        </div>
        <div v-else-if="status === 'error'" class="">Error</div>
        <div v-else class="">Loading...</div>
      </div>
    </section>
  </div>
</template>

<script setup lang="ts">
const id = ref(null);
const selectedGenres = ref([]);
const sortBy = ref("popularity.desc");
const selectedGenresString = computed(() => selectedGenres.value.join("%2C"));

const headers = {
  Authorization:
    "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJiMmYwZWU5MjE3ZDgyOWI3ZGVhYTNiYTI5YTZmODEzYyIsIm5iZiI6MTcyOTk1NzE0My4wNTkyMzUsInN1YiI6IjYyNjI1MmVjMTY4ZWEzMTU1N2QzMzBkMSIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.UVmWHMpoqD0qCekycgjftOuIQHmf6yzZceERpwWlvRs",
  Accept: "application/json",
};
const { data: genres } = await useFetch(
  "https://api.themoviedb.org/3/genre/movie/list?language=en",
  {
    // @ts-ignore
    headers,
    lazy: true,
    server: false,
  }
);

const url = computed(() => {
  return `https://api.themoviedb.org/3/discover/movie?include_adult=false&include_video=false&language=en-US&page=1&sort_by=${sortBy.value}&with_genres=${selectedGenresString.value}`;
});

const { data, status } = await useFetch(url, {
  // @ts-ignore
  headers,
  lazy: true,
  server: false,
  watch: [selectedGenres],
});

const currentSlide = ref(0);
const movies = ref([
  {
    title: "News of the World",
    rating: 7.2,
    year: 2021,
    genre: "Drama",
    description: "A Texan traveling across the wild West bringing the news...",
    image: "path/to/news-of-the-world.jpg",
  },
  {
    title: "Space Sweepers",
    rating: 7.3,
    year: 2021,
    genre: "Sci-Fi",
    description: "When the crew of a space junk collector ship discovers...",
    image: "path/to/space-sweepers.jpg",
  },
  {
    title: "To All the Boys: Always and Forever",
    rating: 8.1,
    year: 2021,
    genre: "Drama",
    description: "Senior year of high school takes center stage as Lara...",
    image: "path/to/to-all-the-boys.jpg",
  },
]);

const setCurrentSlide = (index) => {
  currentSlide.value = index;
};
</script>

<style></style>
