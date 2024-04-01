<template>
    <Modal v-show="isModelOpen" :index="currentOpenImageIndex" :details="productDetails" :type="currentOpenModelType" @closeModel="closeModal" />
    <div class="h-full xl:grid grid-cols-2 xl:py-16 py-8" v-show="!isModelOpen">
      <div class="xl:pr-32 px-[16px]">
        <div class="h-full">
          <imageSlider :thumbnails="productDetails.thumbnails" @lensShow="(val) => isLansShow = val" @openModal="(index, type) => openModal(index , type)" />
        </div>
      </div>
      <div class="xl:pl-10 px-[16px] xl:pt-0 pt-8">
        <div class="h-full">
          <div class="h-full bg-no-repeat" id="resultArea" :class="isLansShow ? 'xl:block hidden' : 'hidden'"></div>
          <ProductDetails :data="productDetails" :class="isLansShow ? 'xl:hidden' : ''" />
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  import img1 from '../assets/images/product-image.jpeg'
  import img2 from '../assets/images/product-image-front-large.jpeg'
  import img3 from '../assets/images/product-image-back.jpeg'
  import vid from '../assets/videos/single-click-zoom-desktop.mp4'

  
  const productDetails = ref({
    brand: 'Samsung',
    inStock: true,
    name: 'Samsung Galaxy S23+ 5G Dual SIM Android Mobile Phone, 512GB (UK Version)',
    price: 650,
    details: {
      sim: ' 5G Dual Sim',
      condition: ' like new',
      color: ' Pink',
      network: ' unlocked',
      storage: ' 512GB'
    },
    thumbnails: [
      { img: img1 },
      { img: img2 },
      { img: img3 },
      { video: vid },
    ],
  });
  
  const isModelOpen = ref(false);
  const currentOpenImageIndex = ref(0);
  const currentOpenModelType = ref(1);
  const isLansShow = ref(false);
  
  const openModal = (index, type) => {
    isModelOpen.value = true;
    currentOpenImageIndex.value = index;
    currentOpenModelType.value = type;
  };
  
  const closeModal = () => {
    isModelOpen.value = false;
    isLansShow.value = false;
  };
  </script>
  
