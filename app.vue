<template>
  <div>
    <NuxtLayout>
      <NuxtPage/>
    </NuxtLayout>
  </div>
</template>

<script setup>
onMounted(() => {
  const body = document.getElementsByTagName('body');
  body[0].setAttribute('class', 'bg-slate-50');
})
</script>
