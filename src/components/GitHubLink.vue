<script setup lang="ts">
defineProps<{
  repo: string
  name?: string
}>()
</script>

<template>
  <span ws-nowrap>
    <CarbonLogoGithub inline-block />
    <a
      op-70 ml-1 font-mono
      :href="`https://github.com/${repo}`"
      target="_blank"
    >
      {{ name || (repo.startsWith('ilyaliao/') ? repo.slice(6) : repo) }}
    </a>
  </span>
</template>
