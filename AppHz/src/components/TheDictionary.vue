<script setup>
import DictionaryItem from './DictionaryItem.vue'

</script>

<template>
  <DictionaryItem>
    <template #icon>
      <DocumentationIcon />
    </template>
    <template #heading>Documentation</template>

    Vue’s
    <a href="https://vuejs.org/" target="_blank" rel="noopener">official documentation</a>
    provides you with all information you need to get started.
  </DictionaryItem>

  <DictionaryItem>
    <template #icon>
      <ToolingIcon />
    </template>
    <template #heading>Tooling</template>
    Vue’s
    <a href="https://vuejs.org/" target="_blank" rel="noopener">official documentation</a>
    provides you with all information you need to get started.
  </DictionaryItem>

  <DictionaryItem>
    <template #icon>
      <EcosystemIcon />
    </template>
    <template #heading>Ecosystem</template>
  </DictionaryItem>

  <DictionaryItem>
    <template #icon>
      <CommunityIcon />
    </template>
    <template #heading>Community</template>
  </DictionaryItem>

  <DictionaryItem>
    <template #icon>
      <SupportIcon />
    </template>
    <template #heading>Support Vue</template>
  </DictionaryItem>
</template>
