<script setup>
defineProps ({
  totalPrice: Number,
  vatPrice: Number
})

import DrawerHead from './DrawerHead.vue';
import CartItemList from './CartItemList.vue';
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
      <div class="bg-white h-full w-96 fixed right-0 top-0 z-20 p-8">


        <DrawerHead />


        <CartItemList />


        <div class="flex flex-col gap-4 mt-7">
          <div class="flex gap-2">
            <span>
              Итого:
            </span>
            <div class="flex-1 border-b border-dashed"></div>
            <b>{{ totalPrice }} Р</b>
          </div>


                    <div class="flex gap-2">
            <span>
              Налог 5%:
            </span>
            <div class="flex-1 border-b border-dashed"></div>
            <b>{{ vatPrice }} Р</b>
          </div>

          <button disabled="" class="mt-4 cursor-pointer transition bg-lime-500 w-full rounded-xl py-4 text-white hover:bg-lime-600 active:bg-lime-700 disabled:bd-slay-300">Оформить заказ</button>
        </div>
    </div>
</template>
