<template>
  <Drawer
    v-if="drawerOpen"
    :totalPrice="totalPrice"
    :vatPrice="vatPrice"
    :buttonDislabled="cartButtonDisabled"
    @create-order="createOrder"
  />
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Header
      :totalPrice="totalPrice"
      @open-drawer="openDrawer"
    />

    <div class="p-10">
      <router-view></router-view>
    </div>
  </div>
</template>

<script setup>
  import { ref, watch, provide, computed } from 'vue';
  import axios from 'axios';

  import Header from '@/components/Header.vue';
  import Drawer from '@/components/Drawer.vue';

  const { VITE_BASE_API: baseApi } = import.meta.env;
  const cart = ref([]);
  const isCreatingOrder = ref(false);
  const drawerOpen = ref(false);

  const totalPrice = computed(() =>
    cart.value.reduce((acc, item) => acc + item.price, 0),
  );

  const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100));

  const cartIsEmpty = computed(() => cart.value.length === 0);

  const cartButtonDisabled = computed(
    () => isCreatingOrder.value || cartIsEmpty.value,
  );

  const openDrawer = () => (drawerOpen.value = true);
  const closeDrawer = () => (drawerOpen.value = false);

  const addToCart = (item) => {
    cart.value.push(item);
    item.isAdded = true;
  };

  const removeFromCart = (item) => {
    cart.value.splice(cart.value.indexOf(item), 1);
    item.isAdded = false;
  };

  const createOrder = async () => {
    try {
      isCreatingOrder.value = true;

      const { data } = await axios.post(`${baseApi}/orders`, {
        items: cart.value,
        totalPrice: totalPrice.value,
      });

      cart.value = [];

      return data;
    } catch (e) {
      console.log(e);
    } finally {
      isCreatingOrder.value = false;
    }
  };

  watch(
    cart,
    () => {
      localStorage.setItem('cart', JSON.stringify(cart.value));
    },
    { deep: true },
  );

  provide('cart', { cart, openDrawer, closeDrawer, addToCart, removeFromCart });
</script>

<style scoped></style>
