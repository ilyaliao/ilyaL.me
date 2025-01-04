<script setup lang="ts">
const { to } = defineProps<{
  to: string
}>()

const isExternalLink = computed(() => {
  return typeof to === 'string' && to.startsWith('http')
})
</script>

<template>
  <a v-if="isExternalLink" v-bind="$attrs" :href="to" target="_blank">
    <slot />
  </a>
  <RouterLink v-else v-bind="$props">
    <slot />
  </RouterLink>
</template>
