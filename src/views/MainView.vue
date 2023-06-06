<template>
  <div>
    <div v-if="showPreloader" class="preloader">
      <div class="preloader__row">
        <div class="preloader__item"></div>
        <div class="preloader__item"></div>
      </div>
    </div>
    <img
        ref="backgroundImage"
        src="/images/background.jpg"
        alt=""
        @load="setImage()"
        class="backgroundImage"
    >
    <ElementComponent
        v-for="element in elements"
        :scale="scale"
        :element-height="element.height"
        :element-width="element.width"
        :left="element.left"
        :top="element.top"
        :color="element.color"
        :description="element.description"
    />
  </div>

</template>

<script>
import ElementComponent from "@/components/ElementComponent.vue";

export default {
  name: 'MainView',
  components: {ElementComponent},
  data() {
    return {
      defaultWidth: 0,
      defaultHeight: 0,
      scale: 100,
      showPreloader: true,
      elements: [
        {height: 40, width: 50, left: 210, top: 42, color: 'red', description: 'Красный элемент'},
        {height: 40, width: 20, left: 900, top: 89, color: 'green', description: 'Зелёный элемент'},
        {height: 20, width: 50, left: 90, top: 600, color: 'blue', description: 'Не красный элемент'},
        {height: 110, width: 98, left: 545, top: 300, color: 'black', description: 'Черный элемент'},
        {height: 200, width: 150, left: 800, top: 400, color: 'white', description: 'Белый элемент'},
      ]
    }
  },
  mounted() {
    this.setResizeEventListener()
  },
  methods: {
    setResizeEventListener() {
      window.addEventListener('resize', (e) => {
        this.setScale();
      });
    },
    setImage() {
      this.defaultWidth = this.$refs.backgroundImage.width
      this.defaultHeight = this.$refs.backgroundImage.height
      this.ratio = this.defaultWidth / this.defaultHeight
      this.setScale()
    },
    setScale() {
      this.showPreloader= true
      let windowWidth = window.innerWidth
      let windowHeight = window.innerHeight

      //Для изображений с шириной больше высоты
      if (windowWidth <= windowHeight || ((windowHeight * this.ratio) > windowWidth)) {
        this.scale = windowWidth / (this.defaultWidth * 0.01) * 0.01
      } else {
        this.scale = (windowHeight * this.ratio) / (this.defaultWidth * 0.01) * 0.01
      }

      this.$refs.backgroundImage.height = this.defaultHeight * this.scale
      this.$refs.backgroundImage.width = this.defaultWidth * this.scale
      this.showPreloader= false
    },
  }
}
</script>

<style scoped>
.backgroundImage {
  position: absolute;
  left: 0;
  top:0
}

.preloader {
  position: fixed;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
  opacity: 0.5;
  z-index: 9;
  background-color: #000;
}

.preloader__row {
  position: relative;
  top: 50%;
  left: 50%;
  width: 70px;
  height: 70px;
  margin-top: -35px;
  margin-left: -35px;
  text-align: center;
  animation: preloader-rotate 2s infinite linear;
}

.preloader__item {
  position: absolute;
  display: inline-block;
  top: 0;
  background-color: #337ab7;
  border-radius: 100%;
  width: 35px;
  height: 35px;
  animation: preloader-bounce 2s infinite ease-in-out;
}

.preloader__item:last-child {
  top: auto;
  bottom: 0;
  animation-delay: -1s;
}

@keyframes preloader-rotate {
  100% {
    transform: rotate(360deg);
  }
}

@keyframes preloader-bounce {

  0%,
  100% {
    transform: scale(0);
  }

  50% {
    transform: scale(1);
  }
}

.loaded_hiding .preloader {
  transition: 0.3s opacity;
  opacity: 0;
}

.loaded .preloader {
  display: none;
}
</style>