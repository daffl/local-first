<script setup lang="ts">
import { ref } from 'vue'
import QrcodeVue, { Level, RenderAs } from 'qrcode.vue'

const props = defineProps({
  value: {
    type: String,
    default: `${window.location.href}#${localStorage.getItem('automerge:repo')}`
  },
})

const value = ref(props.value)
</script>

<template>
  <qrcode-vue class="mx-a h-full" :value="value" :size="500" level="M" render-as="svg" foreground="hsl(248, 38%, 15%)" background="#ffffff" />
</template>