<template>
  <!-- <Drawer /> -->
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Header />

    <div class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>

        <div class="flex gap-4">
          <select class="py-2 px-3 border rounded-md outline-none">
            <option>По названию</option>
            <option>По цене (дешевые)</option>
            <option>По цене (дорогие)</option>
          </select>

          <div class="relative">
            <img
              class="absolute top-2.5 left-4"
              src="/search.svg"
              alt="Search"
            />
            <input
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
  import { onMounted, ref } from 'vue';
  import axios from 'axios';

  import Header from '@/components/Header.vue';
  import CardList from '@/components/CardList.vue';
  // import Drawer from '@/components/Drawer.vue';

  const items = ref([]);

  const { VITE_BASE_API: baseApi } = import.meta.env;

  onMounted(async () => {
    try {
      const { data } = await axios.get(`${baseApi}/items`);

      items.value = data;
    } catch (e) {
      console.log(e);
    }
  });
</script>

<style scoped></style>
