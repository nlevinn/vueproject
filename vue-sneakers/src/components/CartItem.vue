<script setup>
  defineProps({
    id: Number,
    title: String,
    imageUrl: String,
    price: Number
  })
</script>


<template>
  <div class=" border border-slate-200 p-4 rounder-xl flex items-center gap-4">
    <img class="w-16 h-16" :src="imageUrl" :alt="title">

    <div class="flex flex-col flex-1">
      <p>{{ title }}</p>

      <div class="flex justify-between mt-2">
        <b class="flex-1">{{ price }} руб.</b>
        <img class="opacity-40 cursor-pointer hover:opacity-100 transition" src="/close.svg" alt="close">
      </div>
    </div>
  </div>
</template>
