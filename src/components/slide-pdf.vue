<template>
  <div class="wrap">
    <div
      class="carousel"
      :style="{ transform: `translateX(${currentOffset}px)` }"
      @mousedown="startDrag"
      @mousemove="onDrag"
      @mouseup="endDrag"
      @mouseleave="endDrag"
      @touchstart="startDrag"
      @touchmove="onDrag"
      @touchend="endDrag"
    >
      <div v-for="(page, index) in imgArr" :key="index" class="item">
        <img :src="page" :alt="`Image ${index}`" />
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, onMounted } from "vue";

const imgArr = ref<string[]>([]);
const currentIndex = ref(0); // Chỉ số hình hiện tại
const currentOffset = ref(0); // Giá trị translateX của carousel
let startX = 0; // Lưu vị trí bắt đầu của kéo/ vuốt
let isDragging = false; // Đánh dấu trạng thái có đang kéo hay không

onMounted(() => {
  for (let i = 1; i < 232; i++) {
    imgArr.value.push(`/assets/source-${i}.jpg`);
  }
});

// Bắt đầu kéo hoặc vuốt
const startDrag = (event: MouseEvent | TouchEvent) => {
  isDragging = true;
  startX =
    event instanceof MouseEvent ? event.clientX : event.touches[0].clientX;
};

// Xử lý sự kiện kéo hoặc vuốt
const onDrag = (event: MouseEvent | TouchEvent) => {
  if (!isDragging) return;

  const currentX =
    event instanceof MouseEvent ? event.clientX : event.touches[0].clientX;
  const deltaX = currentX - startX;

  // Cập nhật giá trị translateX khi kéo hoặc vuốt
  currentOffset.value = deltaX - currentIndex.value * window.innerWidth;
};

// Kết thúc kéo hoặc vuốt, xác định có chuyển trang hay không
const endDrag = () => {
  if (!isDragging) return;
  isDragging = false;

  const threshold = 100; // Ngưỡng để xác định có chuyển trang
  const delta = currentOffset.value + currentIndex.value * window.innerWidth;

  if (delta > threshold) {
    prevImage(); // Kéo hoặc vuốt sang phải => lùi lại
  } else if (delta < -threshold) {
    nextImage(); // Kéo hoặc vuốt sang trái => tiến tới
  } else {
    currentOffset.value = -currentIndex.value * window.innerWidth; // Trả về vị trí ban đầu nếu không đủ ngưỡng
  }
};

// Chuyển đến hình tiếp theo
const nextImage = () => {
  if (currentIndex.value < imgArr.value.length - 1) {
    currentIndex.value++;
    currentOffset.value = -currentIndex.value * window.innerWidth;
  }
};

// Quay về hình trước đó
const prevImage = () => {
  if (currentIndex.value > 0) {
    currentIndex.value--;
    currentOffset.value = -currentIndex.value * window.innerWidth;
  }
};
</script>

<style scoped>
.wrap {
  height: 100vh;
  width: 100vw;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
}

.carousel {
  display: flex;
  transition: transform 0.3s ease-in-out; /* Hiệu ứng chuyển trang mượt mà */
  width: 100vw;
}

.item {
  min-width: 100vw; /* Mỗi ảnh chiếm toàn bộ màn hình */
  display: flex;
  align-items: center;
  justify-content: center;
}

img {
  width: 100%;
  height: auto;
}
</style>
