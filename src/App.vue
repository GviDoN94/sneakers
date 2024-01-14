<template>
  <!-- <Drawer /> -->
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Header />

    <div class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>

        <div class="flex gap-4">
          <select
            @change="onChangeSelect"
            class="py-2 px-3 border rounded-md outline-none"
          >
            <option value="name">По названию</option>
            <option value="price">По цене (дешевые)</option>
            <option value="-price">По цене (дорогие)</option>
          </select>

          <div class="relative">
            <img
              class="absolute top-2.5 left-4"
              src="/search.svg"
              alt="Search"
            />
            <input
              @change="onChangeSearchInput"
              type="text"
              placeholder="Поиск..."
              class="border rounded-md py-1.5 pl-11 pr-4 outline-none focus:border-gray-400"
            />
          </div>
        </div>
      </div>

      <div class="mt-10">
        <CardList :items="items" />
      </div>
    </div>
  </div>
</template>

<script setup>
  import { onMounted, ref, reactive, watch } from 'vue';
  import axios from 'axios';

  import Header from '@/components/Header.vue';
  import CardList from '@/components/CardList.vue';
  // import Drawer from '@/components/Drawer.vue';

  const { VITE_BASE_API: baseApi } = import.meta.env;
  const items = ref([]);

  const filters = reactive({
    sortBy: 'title',
    searchQuery: '',
  });

  const onChangeSelect = (event) => (filters.sortBy = event.target.value);

  const onChangeSearchInput = (event) =>
    (filters.searchQuery = event.target.value);

  const fetchFavorites = async () => {
    try {
      const { data: favorites } = await axios.get(`${baseApi}/favorites`);

      items.value = items.value.map((item) => {
        const favorite = favorites.find(
          (favorite) => favorite.parentId === item.id,
        );

        if (!favorite) {
          return item;
        }

        return {
          ...item,
          isFavorite: true,
          favoriteId: favorites.id,
        };
      });
    } catch (e) {
      console.log(e);
    }
  };

  const addToFavorite = async (item) => {
    item.isFavorite = true;
  };

  const fetchItems = async () => {
    try {
      const params = {
        sortBy: filters.sortBy,
      };

      if (filters.searchQuery) {
        params.title = `*${filters.searchQuery}*`;
      }

      const { data } = await axios.get(`${baseApi}/items`, { params });

      items.value = data.map((obj) => ({
        ...obj,
        isFavorite: false,
        isAdded: false,
      }));
    } catch (e) {
      console.log(e);
    }
  };

  onMounted(async () => {
    await fetchItems();
    await fetchFavorites();
  });
  watch(filters, fetchItems);
</script>

<style scoped></style>
