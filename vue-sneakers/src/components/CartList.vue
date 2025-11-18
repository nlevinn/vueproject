<script setup>

import Cart from './Cart.vue';

defineProps({
  items: Array,
  isFavorites: Boolean
})


const emit = defineEmits(['addToFavorite', 'addToCart'])



</script>

<template>
    <div class="grid grid-cols-4 gap-5">
<Cart
v-for="item in items"
:key="item.id"
:id="item.id"
:image-url="item.imageUrl"
:title="item.title"
:price="item.price"
:onClickFavorite="() => emit('addToFavorite', item)"
:onClickAdd="() => emit('addToCart', item)"
:isFavorite="item.isFavorite"
:isAdded="item.isAdded"
/>
  </div>
</template>
