<template>
  <div class="container mt-5">
    <h5>ขนาด</h5>
    <div class="btn-group" role="group" aria-label="Size options">
      <button
        :class="{ active: selectedSize === 'vertical' }"
        @click="selectedSize = 'vertical'"
      >
        แนวตั้ง
      </button>
      <button
        :class="{ active: selectedSize === 'horizontal' }"
        @click="selectedSize = 'horizontal'"
      >
        แนวนอน
      </button>
      <button
        :class="{ active: selectedSize === 'square' }"
        @click="selectedSize = 'square'"
      >
        จตุรัส
      </button>
    </div>
    
    <h5 class="mt-4">โทนสี</h5>
    <div class="d-flex flex-wrap gap-2">
      <button
        v-for="color in colors"
        :key="color"
        class="btn color-circle"
        :style="{ backgroundColor: color }"
        :class="{ 'border-primary': selectedColor === color }"
        @click="selectedColor = color"
      ></button>
    </div>

    <h5 class="mt-4">สไตล์ภาพ</h5>
    <div class="image-gallery">
      <div
        v-for="(image, index) in visibleImages"
        :key="index"
        class="col image-col"
      >
        <div
          class="image-item"
          :style="{
            backgroundColor: selectedColor,
            width: getSizeStyle(selectedSize).width,
            height: getSizeStyle(selectedSize).height
          }"
        >
          <img :src="image.url" :alt="image.style" class="image-img" />
          <div class="image-overlay">
            <div class="image-style">
              <button
                v-for="style in image.styles"
                :key="style"
                :class="{ 'active': selectedStyle === style }"
                @click="selectStyle(style)"
                class="style-button"
              >
                {{ style }}
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="text-center mt-4">
      <button class="btn btn-link" @click="loadMore">โหลดเพิ่ม</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectedSize: 'square',
      selectedColor: '#ffffff',
      selectedStyle: '',
      colors: [
        '#ff0000', '#00ff00', '#0000ff', '#ffff00', '#ff00ff', '#00ffff',
        '#000000', '#ffffff', '#808080', '#800000', '#008000', '#000080',
        '#d3d3d3', '#ffd700', '#c0c0c0', '#f5f5dc', '#f0e68c', '#4682b4',
        '#006400', '#ff69b4', '#ffa500', '#8b4513', '#800080', '#ff6347'
      ],
      images: [
        { url: '/images/img1.png', styles: ['vintage'] },
        { url: '/images/img2.jpg', styles: ['modern'] },
        { url: '/images/img3.png', styles: ['anime'] },
        { url: '/images/img4.jpg', styles: ['abstract'] },
        { url: '/images/img5.png', styles: ['minimalist'] },
        { url: '/images/img6.png', styles: ['retro'] },
        { url: '/images/img7.png', styles: ['classic'] },
        { url: '/images/img8.png', styles: ['grunge'] },
        { url: '/images/img9.jpg', styles: ['pop'] },
        { url: '/images/img10.jpg', styles: ['cyberpunk'] },
        { url: '/images/img11.jpg', styles: ['gothic'] },
        { url: '/images/img12.jpg', styles: ['fantasy'] },
        { url: '/images/img13.jpg', styles: ['surreal'] },
        { url: '/images/img14.jpg', styles: ['impressionist'] },
        { url: '/images/img15.png', styles: ['expressionist'] },
        { url: '/images/img16.jpg', styles: ['steet'] },
        { url: '/images/img17.jpg', styles: ['sci-fi'] },
        { url: '/images/img18.png', styles: ['romance'] },
        { url: '/images/img19.jpg', styles: ['fantasy'] },
        { url: '/images/img20.jpg', styles: ['3d'] },
      ],
      visibleImages: [],
      visibleImageCount: 12
    };
  },
  created() {
    this.loadMore();
  },
  computed: {
    getGridStyle() {
      return this.selectedSize === 'horizontal'
        ? { 'grid-template-columns': 'repeat(auto-fill, minmax(300px, 1fr))' }
        : { 'grid-template-columns': 'repeat(auto-fit, minmax(300px, 1fr))' }; 
    }
  },
  methods: {
    getSizeStyle(size) {
      switch (size) {
        case 'vertical':
          return { width: '250px', height: '500px' };
        case 'horizontal':
          return { width: '300px', height: '150px' };
        case 'square':
          return { width: '300px', height: '300px' };
        default:
          return { width: '300px', height: '300px' };
      }
    },
    loadMore() {
      const newImages = this.images.slice(0, this.visibleImageCount);
      this.visibleImages = [...this.visibleImages, ...newImages];
      this.visibleImageCount += 12;
    },
    selectStyle(style) {
      this.selectedStyle = style;
    }
  }
};
</script>

<style scoped>
.container {
  padding: 20px;
}

.btn-group button {
  cursor: pointer;
  border: none;
  margin: 5px;
}

.btn-group button.active {
  background-color: #ddd;
}

.btn.color-circle {
  width: 40px;
  height: 40px;
  border-radius: 50%;
}

.btn.color-circle.border-primary {
  border: 2px solid #0d6efd;
}

.image-gallery {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 50px;
}

.image-item {
  position: relative;
  overflow: hidden;
  border: 2px solid #ddd;
  border-radius: 10px;
  height: 300px;
}
.image-col {
  padding: 0;
}

.image-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.image-overlay {
  position: absolute;
  bottom: 10px;
  left: 50%;
  transform: translateX(-50%);
  text-align: center;
  padding: 5px;
  border-radius: 0 0 10px 10px;
}

.image-style {
  display: flex;
  justify-content: center;
  gap: 5px;
}

.style-button {
  border: none;
  background-color: #eee;
  padding: 5px 10px;
  cursor: pointer;
  border-radius: 10px;
  border: 2px solid #aaa;
  width: 100%;
  text-align: center; 
}

.style-button.active {
  background-color: #e0f7fa;
  border-color: #81d4fa;
}
</style>
