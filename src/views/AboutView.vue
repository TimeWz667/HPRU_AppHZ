<script setup>
import TheDictionary from '@/components/TheDictionary.vue'

defineProps({
  msg: {
    type: String,
    required: true,
  },
})
</script>

<template>
  <div class="about">
    <the-dictionary/>
  </div>
</template>

<style>

</style>
