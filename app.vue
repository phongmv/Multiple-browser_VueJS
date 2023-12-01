<template>
    <div class="container">
      <div class="content">
        <img src="~/assets/img/songoku.jpeg" alt="...">
      </div>
    </div>

</template>
<script setup>
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
.container {
  position: relative;
}

.content{
  transition: all 1s linear;
  position: absolute;
  top: 0;
  left: 0;
  width: 200px;
  height: 400px;
  object-fit: contain;
}
img {
  width: 100%;
  height: 100%;
}

</style>
