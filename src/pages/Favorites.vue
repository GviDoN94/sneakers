<template>
  <h2 class="text-3xl font-bold mb-8">Мои закладки</h2>

  <CardList
    :items="favorites"
    @add-to-favorite="deleteFromFavorite"
    @add-to-cart="onClickAddPlus"
  />
</template>

<script setup>
  import { ref, onMounted, inject } from 'vue';
  import axios from 'axios';
  import CardList from '@/components/CardList.vue';

  const { VITE_BASE_API: baseApi } = import.meta.env;

  const { cart, onClickAddPlus } = inject('cart');

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
        Object.assign({}, obj.item, {
          isFavorite: true,
          favoriteId: obj.id,
          isAdded: cart.value.some((cartItem) => cartItem.id === obj.item.id),
        }),
      );
    } catch (e) {
      console.log(e);
    }
  });
</script>
