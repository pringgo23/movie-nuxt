<template>
  <div class="bg-gradient h-fit pb-8 text-white w-64 rounded-lg shadow-md">
    <div class="mb-6">
      <label class="block text-lg font-semibold mb-2 px-4 pt-5 pb-4">Sort Result By</label>
      <div id="border-solid" class="h-0.5 border-t border-solid border-white border-opacity-10" />
      <div class="px-3 pt-4 pb-7">
          <select
            class="w-11/12 bg-gray-700 text-white p-3 rounded border border-gray-600 focus:outline-none focus:border-gray-400"
            :value="sortBy"
            @input="$emit('update:sortBy', $event.target.value)"
          >
            <option value="popularity.asc">Popularity Ascending</option>
            <option value="popularity.desc">Popularity Descending</option>
            <option value="release_date.asc">Release Date Ascending</option>
            <option value="release_date.desc">Release Date Descending</option>
            <option value="vote_average.asc">Rating Ascending</option>
            <option value="vote_average.desc">Rating Descending</option>
          </select>
      </div>
    </div>
    <div id="border-solid" class="h-0.5 border-t border-solid border-white border-opacity-10" />
    <div>
      <h3 class="text-lg font-semibold p-4">Genres</h3>
      <div id="border-solid" class="h-0.5 border-t border-solid border-white border-opacity-10 mb-3" />
      <div
        v-for="item in genres.genres"
        :key="item.id"
        class="flex space-y-2 w-48 justify-between items-center space-x-2 px-4"
      >
        <label :for="item.id" class="text-gray-200">{{ item.name }}</label>
        <input
          type="checkbox"
          :id="item.id"
          class="custom-checkbox"
          :value="selectedGenres"
          :checked="isChecked(item.id)"
          @change="toggleGenre(item.id)"
        />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
interface Genre {
  id: number;
  name: string;
}

// @ts-ignore
const props = defineProps<{
  selectedGenres: any;
  sortBy: string;
  genres: Genre;
}>();

function isChecked(id: number) {
  return props.selectedGenres.includes(id);
}

function toggleGenre(id: number) {
  const newSelectedGenres = [...props.selectedGenres];

  if (newSelectedGenres.includes(id)) {
    // If genre ID is already selected, remove it
    newSelectedGenres.splice(newSelectedGenres.indexOf(id), 1);
  } else {
    // Otherwise, add the genre ID
    newSelectedGenres.push(id);
  }
  emit("update:selectedGenres", newSelectedGenres);
}

// @ts-ignore
const emit = defineEmits(["update:sortby", "update:selectedGenres"]);
</script>

<style>

.custom-checkbox {
  @apply appearance-none w-5 h-5 border-2 border-gray-400 rounded-md cursor-pointer transition-all duration-300 ease-in-out;
}

.custom-checkbox:checked {
  @apply bg-[#E74C3C] border-transparent;
}

.custom-checkbox:checked::before {
  content: '✔';
  color: white;
  font-size: 0.75rem;
  text-align: center;
  display: block;
  line-height: 1.1rem; /* Centering the checkmark */
}

.bg-gradient {
  background: linear-gradient(to bottom, #1a1a1a, #2a2a2a08);
}
</style>
