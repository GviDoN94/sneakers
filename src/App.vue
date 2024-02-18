<template>
  <Drawer
    v-if="drawerOpen"
    :totalPrice="totalPrice"
    :vatPrice="vatPrice"
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
  import { ref, watch, provide, computed, onMounted } from 'vue';

  import Header from '@/components/Header.vue';
  import Drawer from '@/components/Drawer.vue';

  const cart = ref([]);
  const drawerOpen = ref(false);

  const totalPrice = computed(() =>
    cart.value.reduce((acc, item) => acc + item.price, 0),
  );

  const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100));

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

  const onClickAddPlus = (item) => {
    if (!item.isAdded) {
      addToCart(item);
    } else {
      removeFromCart(item);
    }
  };

  onMounted(async () => {
    const localCart = localStorage.getItem('cart');
    cart.value = localCart ? JSON.parse(localCart) : [];
  });

  watch(
    cart,
    () => {
      localStorage.setItem('cart', JSON.stringify(cart.value));
    },
    { deep: true },
  );

  provide('cart', { cart, openDrawer, closeDrawer, onClickAddPlus });
</script>

<style scoped></style>
