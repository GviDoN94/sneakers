<template>
  <div class="flex items-center border border-slate-100 p-4 rounded-xl gap-4">
    <img
      class="w-16 h-16 object-contain"
      :src="imageUrl"
      alt="Sneaker"
    />

    <div class="flex flex-col">
      <p>Кроссовки Nike Air Max 270</p>

      <div class="flex justify-between mt-2">
        <b>12990 руб.</b>
        <img
          class="opacity-30 hover:opacity-100 cursor-pointer transition"
          src="/close.svg"
          alt="Close"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
  defineProps({
    imageUrl: String,
  });
</script>
