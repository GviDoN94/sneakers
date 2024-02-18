<template>
  <div
    @click="closeDrawer"
    class="fixed top-0 left-0 w-full h-full bg-black/70 z-10"
  ></div>
  <div class="flex flex-col bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
    <DrawerHead />

    <div
      v-if="!totalPrice || orderId"
      class="flex h-full items-center"
    >
      <InfoBlock
        v-if="!totalPrice && !orderId"
        title="Корзина пустая"
        description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
        imageUrl="/package-icon.png"
      />

      <InfoBlock
        v-if="orderId"
        title="Заказ оформлен!"
        :description="`Ваш заказ #${orderId} скоро будет передан курьерской службе`"
        imageUrl="/order-success-icon.png"
      />
    </div>

    <div
      v-else
      class="flex flex-col h-full"
    >
      <CartItemList />

      <div class="flex flex-col gap-4 mt-7">
        <div class="flex gap-2">
          <span>Итого:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ totalPrice }} руб.</b>
        </div>

        <div class="flex gap-2">
          <span>Налог 5%:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ vatPrice }} руб.</b>
        </div>

        <button
          :disabled="buttonDisabled"
          class="mt-4 transition bg-lime-500 w-full rounded-xl py-3 text-white disabled:bg-slate-300 hover:bg-lime-600 active:bg-lime-700 cursor-pointer"
          @click="createOrder"
        >
          Оформить заказ
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
  import { ref, computed, inject } from 'vue';
  import axios from 'axios';

  import DrawerHead from '@/components/DrawerHead.vue';
  import CartItemList from '@/components/CartItemList.vue';
  import InfoBlock from './infoBlock.vue';

  const props = defineProps({
    totalPrice: Number,
    vatPrice: Number,
  });

  const { VITE_BASE_API: baseApi } = import.meta.env;

  const { cart, closeDrawer } = inject('cart');
  const isCreatingOrder = ref(false);
  const orderId = ref(null);

  const cartIsEmpty = computed(() => cart.value.length === 0);

  const buttonDisabled = computed(
    () => isCreatingOrder.value || cartIsEmpty.value,
  );

  const createOrder = async () => {
    try {
      isCreatingOrder.value = true;

      const { data } = await axios.post(`${baseApi}/orders`, {
        items: cart.value,
        totalPrice: props.totalPrice.value,
      });

      cart.value = [];

      orderId.value = data.id;
    } catch (e) {
      console.log(e);
    } finally {
      isCreatingOrder.value = false;
    }
  };
</script>
