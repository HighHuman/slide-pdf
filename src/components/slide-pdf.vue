<template>
  <div class="wrap">
    <Carousel>
      <Slide v-for="(page, index) in imgArr" :key="index">
        <img :src="page" :alt="`${index}`" />
      </Slide>
    </Carousel>
  </div>
</template>

<script lang="ts" setup>
import { ref, onMounted } from "vue";

import { Carousel, Slide } from "vue3-carousel";
const imgArr = ref<string[]>([]);

onMounted(() => {
  for (let i = 1; i < 232; i++) {
    imgArr.value.push("/assets/source-" + i + ".jpg");
  }
});
</script>
<style scoped>
.wrap {
  height: 100vh;
  width: 100vw;
  display: flex;
  align-items: center;
  justify-content: center;
}
.carousel__item {
  height: 95vh;
  width: 95vw;
}
.carousel,
.carousel__viewport {
  height: 100vh;
  width: 100vw;
}

img {
  object-fit: cover;
  max-height: 98vh;
  max-width: 98vw;
}
.carousel {
  display: flex;
  align-items: center;
}
</style>
