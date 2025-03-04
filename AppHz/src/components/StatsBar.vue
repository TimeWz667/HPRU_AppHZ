<script>
export default {
  props: {
    stat: Object,
  },
  data() {
    return {
      width: '100%',
      height: '20',
      scale: 1
    }
  },
  mounted() {
    this.width = document.getElementById('bar_' + this.stat.Key).getBoundingClientRect().width;
    this.scale = this.width / this.stat.upper;
  },
  computed: {
    y0() {
      return this.width * this.stat.Y0 / this.stat.upper
    },
    y1() {
      return this.width * this.stat.Y1 / this.stat.upper
    }
  }
}
</script>

<template>
  <div :id="'bar_' + stat.Key" class="bar">
<!--    {{ stat }}-->
    <svg :height="height * 2" width="100%">
      <rect x="0" y="0" :width="y0" :height="height" fill="#111111" opacity="0.6" />
      <text :x="y0 + 10" :y="height / 2" dy="0.4em">{{this.stat.fmt(this.stat.Y0)}}</text>
      <rect x="0" :y="height" :width="y1" :height="height" fill="#1114aa" opacity="0.6" />
      <text :x="y1 + 10" :y="height * 3 / 2" dy="0.4em">{{this.stat.fmt(this.stat.Y1)}}</text>
    </svg>
  </div>
</template>

<style scoped></style>
