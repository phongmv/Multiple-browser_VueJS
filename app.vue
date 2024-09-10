<template>
    <div class="container">
      <div class="examples">
        <h2 class="des-title">Duplicate your browser windows, exit full-screen mode, and resize the browser. </h2>
        <h3 class="des-title">Examples:</h3>
        <div :class="{'zoomed': isZoomed}" class="image-container" @click="toggleZoom">
          <NuxtImg
              :class="{'zoomed': isZoomed}"
              class="zoom-image"
              src="/des_image.png"
              alt="des_image"
              :width="{zoomedWidth}"
              :height="{zoomedHeight}"/>
        </div>
      </div>
      <div class="content">
        <NuxtImg
            src="/songoku.png"
            alt="Main image"
            width="200"
            height="400"/>
      </div>
    </div>
  <NuxtParticles
      id="tsparticles"
      :options="options"
  />
</template>
<script setup>
import { ref } from 'vue';

const options = {
  fullScreen: {
    enable: true,
    zIndex: -1
  },
  particles: {
    color: {
      value: "#ffffff" // Color of the stars
    },
    links: {
      color: "#ffffff",
      enable: false // Disable links for a cleaner galaxy effect
    },
    move: {
      enable: true,
      speed: 0.2 // Slower speed for a starry background
    },
    number: {
      value: 200 // Number of stars
    },
    opacity: {
      value: {
        min: 0.1,
        max: 0.7 // Varying opacity for depth
      }
    },
    size: {
      value: {
        min: 1,
        max: 3 // Varying size for a more realistic effect
      }
    },
    shape: {
      type: "circle" // Star shape
    },
    density: {
      enable: true,
      value_area: 800 // Control particle density
    }
  },
  background: {
    color: {
      value: "#000000" // Dark background to simulate space
    }
  }
};

const isZoomed = ref(false);
const zoomedWidth = ref(100)
const zoomedHeight = ref(100)
const toggleZoom = () => {
  isZoomed.value = !isZoomed.value;
  isZoomed.value ? zoomedWidth.value = 400 : zoomedWidth.value = 100
  isZoomed.value ? zoomedHeight.value = 400 : zoomedHeight.value = 100
};

onMounted(() => {
  tick()
})

const tick = () => {
  requestAnimationFrame(() => {
    getTransform()
    tick()
  })
}

const getTransform = () => {
  const windowDetails = {
    screenX: window.screenX,
    screenY: window.screenY,
    screenWidth: window.screen.availWidth,
    screenHeight: window.screen.availHeight,
  };
  const content = document.querySelector('.content')
  if (!content) {
    return;
  }
  const { screenX, screenY, screenWidth, screenHeight } = windowDetails;
  const contentWidth = content.clientWidth;
  const contentHeight = content.clientHeight;
  const screenCenterX = screenWidth / 2;
  const screenCenterY = screenHeight / 2;
  const contentX = screenCenterX - (contentWidth / 2);
  const contentY = screenCenterY - (contentHeight / 2);
  content.style.transform = `translate(${-(screenX - contentX)}px, ${-(screenY - contentY)}px)`
}

</script>

<style  scoped>

.content{
  transition: all 0.1s linear;
  position: absolute;
  top: 0;
  left: 0;
}

.container {
  position:  relative;
  z-index: 99;
}

h2, h3{
  font-size: 14px;
  color: white;
}


.image-container {
  display: inline-block;
  position: relative;
  overflow: hidden;
  width: 100px;
  height: 100px;
}

.image-container.zoomed {
  width: 400px;
  height: 400px;
  object-fit: contain;
}

.zoom-image {
  transition: transform 0.3s ease;
  cursor: pointer;
  width: 100%;
  height: auto;
}

.zoom-image.zoomed {
  transition: 0.2s all linear;
  transform: scale(1.4);
}

</style>
