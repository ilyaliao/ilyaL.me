<script setup lang="ts">
const { text, slice } = defineProps<{
  text?: string
  slice?: [number, number]
}>()

const { copy: _copy, copied } = useClipboard()
const el = ref<HTMLElement | null>(null)

function copy() {
  _copy(
    text ?? (el.value?.textContent || '').trim().slice(...(slice || [0])),
  )
}
</script>

<template>
  <div ref="el" gap-1 items-center>
    <slot />
    <button
      title="Copy" inline ml-2 op-30 hover:op-100 text-sm transition
      :class="copied ? 'i-carbon-checkmark text-green' : 'i-carbon-copy'" @click="copy()"
    />
  </div>
</template>
