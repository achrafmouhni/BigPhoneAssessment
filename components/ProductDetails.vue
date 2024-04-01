<template>
    <div>
      <div class="flex items-center justify-between">
        <p class="md:text-[18px] text-base font-normal text-slate-400 uppercase">{{ data.brand }}</p>
      </div>
  
      <h1 class="md:text-[32px] text-[24px] font-semibold my-2 capitalize">{{ data.name }}</h1>
      <p class="md:text-[24px] text-[20px] font-bold">{{ realPrice }}</p>
      <hr class="my-4 border-2  border-[#6082B6] text-white">
  
      <div v-for="(details, index) in Object.entries(data.details)" :key="index">
        <p class="md:text-lg text-base py-2">
          <strong class="capitalize">{{ details[0] }}:</strong>
          <span class="uppercase tracking-wide">{{ details[1] }}</span>
        </p>
      </div>
      <hr class="my-4 border  border-[#6082B6] text-white">
  
      <button @click="handleButtonClick" :class="{ 'bg-yellow-400': !buttonClicked, 'bg-green-400': buttonClicked }" class="w-full py-2 rounded font-bold text-xl capitalize text-white">
        {{ buttonClicked ? 'Added To Basket' : 'Add To Basket' }}
      </button>
      <hr class="my-4 border border-[#6082B6] text-white">
      <p>The {{ data.brand }} {{ data.name }} is a flagship smartphone that belongs to the {{ data.brand }} {{ data.series }} series, offering users a premium experience with cutting-edge technology and a sleek design.</p>
  
    </div>
  </template>
  
  <script setup>
  import { defineProps, ref } from 'vue';
  
  const props = defineProps({
    data: {
      required: true,
    }
  });
  
  const realPrice = ref(myPrice(props.data.price));
  const buttonClicked = ref(false);
  
  function myPrice(price) {
    return '£' + price.toFixed(2);
  }
  
  function handleButtonClick() {
    buttonClicked.value = true;
    
  }
  </script>
  