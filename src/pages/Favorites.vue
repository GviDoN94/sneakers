<template>
  <h1>Мои закладки</h1>

  <CardList
    :items="favorites"
    @add-to-favorite="deleteFromFavorite"
  />
</template>

<script setup>
  import { ref, onMounted } from 'vue';
  import axios from 'axios';
  import CardList from '@/components/CardList.vue';

  const { VITE_BASE_API: baseApi } = import.meta.env;

  const favorites = ref([]);

  const deleteFromFavorite = async (item) => {
    try {
      favorites.value = favorites.value.filter(
        (sneakers) => sneakers.favoriteId !== item.favoriteId,
      );

      await axios.delete(`${baseApi}/favorites/${item.favoriteId}`);
    } catch (e) {
      console.log(e);
    }
  };

  onMounted(async () => {
    try {
      const { data } = await axios.get(`${baseApi}/favorites?_relations=items`);

      favorites.value = data.map((obj) =>
        Object.assign({}, obj.item, { isFavorite: true, favoriteId: obj.id }),
      );
    } catch (e) {
      console.log(e);
    }
  });
</script>
