<script setup>
import { onMounted, ref, reactive, provide,  watch, computed } from 'vue';
import axios from 'axios'

import Header from './components/Header.vue';
import CartList from './components/CartList.vue';
import Drawer from './components/Drawer.vue';


const items = ref([]);
const cart = ref([]);

const drawerOpen = ref(false);

const totalPrice = computed(
  () => cart.value.reduce((acc, item) => acc + item.price, 0)
);

const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100))

const closeDrawer = () => {
  drawerOpen.value = false
}

const openDrawer = () => {
  drawerOpen.value = true
}

const addToCart = (item) => {
      cart.value.push(item)
    item.isAdded = true
}

const removeFromCart = (item) => {
      cart.value.splice(cart.value.indexOf(item), 1)
    item.isAdded = false
}

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item);
  }else{
    removeFromCart(item);
  }
}

const filters = reactive({
    sortBy: 'title',
  searchQuery: ''

});


const onChangeSelect = event => {
  filters.sortBy = event.target.value;
}

const onChangeSearchInput = event => {
  filters.searchQuery = event.target.value;
}

const fetchFavorites = async () => {
  try {
    const {data: favorites} = await axios.get('https://197f6b753459f044.mokky.dev/favorites');

    items.value = items.value.map(item => {
      const favorite = favorites.find((favorite) => favorite.parentId === item.id);
      if (!favorite) {
        return item
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      };
    });
  } catch (err) {
    console.log(err)
  }
}

const addToFavorite = async (item) => {
  try {

    if(!item.isFavorite) {
      const obj = {
      parentId: item.id
    };
    const {data} = await axios.post(`https://197f6b753459f044.mokky.dev/favorites`, obj);
    item.isFavorite = true;
    item.favoriteId = data.id;
    } else {
      await axios.delete(`https://197f6b753459f044.mokky.dev/favorites/${item.favoriteId}`)
      item.isFavorite = false
      item.favoriteId = null
    }

  } catch (err) {
    console.log(err)
  }
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if(filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const {data} = await axios.get('https://197f6b753459f044.mokky.dev/items', {
      params
    });

    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false
    }));
  } catch (err) {
    console.log(err)
  }
}

onMounted(async () => {
  await fetchItems();
  await fetchFavorites();
})
watch(filters, fetchItems);

provide('cart', {
  cart,
  closeDrawer,
  openDrawer,
  addToCart,
  removeFromCart
});

</script>

<template>
  <Drawer v-if="drawerOpen" :total-price="totalPrice" :vat-price="vatPrice"/>


<div class="w-4/5 m-auto bg-white  rounded-xl shadow-xl mt-14 mb-14">
  <Header :total-price="totalPrice"  @open-drawer="openDrawer" />

<div class="p-10">
  <div class="flex justify-between items-center">
    <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>

<div class="flex gap-4">
  <select @change="onChangeSelect" class="py-2 px-3 border border-gray-200 rounded-md outline-none">
  <option value="name">
    По названию
  </option>
    <option value="price">
    По цене (дешевые)
  </option>
    <option value="-price">
    По цене (дорогие)
  </option>
</select>

    <div class="relative">
      <img src="/search.svg" alt="search" class="absolute left-4 top-3">
      <input @input="onChangeSearchInput" placeholder="Поиск...." class="border border-gray-200 rounded-md py-2 pl-11 pr-4 outline-none focus:border-grey-400">
    </div>
  </div>

</div>
<div class="mt-10">
  <CartList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus"/>
</div>
</div>


</div>
</template>
