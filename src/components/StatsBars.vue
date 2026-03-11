<script setup>
import StatsBar from './StatsBar.vue'

const props = defineProps({
  stats: Array
})
</script>

<template>
  <div id="stats_set">
    <div v-for="stat in props.stats">
      <h5>{{stat.Desc}}</h5>
      <StatsBar :stat="stat" />
    </div>
  </div>
</template>

<style scoped>
#stats_set {
  width: 100%;
}
</style>
