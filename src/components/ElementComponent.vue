<template>
  <div class="element" :style="getStyle()" @click="tooltipShow= !tooltipShow">
    <span v-if="tooltipShow" class="tooltip">{{ description }}</span>
  </div>
</template>

<script>

export default {
  name: 'ElementComponent',
  props: {
    scale: {
      required: true,
      type: Number
    },
    top: {
      required: true,
      type: Number
    },
    left: {
      required: true,
      type: Number
    },
    elementWidth: {
      required: true,
      type: Number
    },
    elementHeight: {
      required: true,
      type: Number
    },
    color: {
      required: true,
      type: String
    },
    description: {
      required: true,
      type: String
    }
  },
  data() {
    return {
      tooltipShow: false
    }
  },
  methods: {
    getStyle() {
      let width = this.elementWidth * this.scale
      let height = this.elementHeight * this.scale
      let top = this.top * this.scale
      let left = this.left * this.scale

      return `width:${width}px; height:${height}px; top:${top}px; left:${left}px; background-color:${this.color}`
    },
  }
}
</script>

<style scoped>
.element {
  position: absolute;
  cursor: pointer;
}

.element .tooltip {
  width: 120px;
  background-color: black;
  color: #fff;
  text-align: center;
  padding: 5px 5px;
  border-radius: 6px;
  position: absolute;
  z-index: 1;
  cursor: pointer;

}
</style>