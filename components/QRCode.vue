<script setup lang="ts">
import { ref } from 'vue'
import QrcodeVue, { Level, RenderAs } from 'qrcode.vue'

const props = defineProps({
  value: {
    type: String
  },
  page: {
    type: String,
  }
})

const hash = localStorage.getItem('automerge:repo')
const defaultValue = props.value || props.page ?
  `${window.location.protocol}//${window.location.host}/${props.page}#${hash}` : `${window.location.href}#${hash}`
const value = ref(defaultValue)
</script>

<template>
  <a :href="value"><qrcode-vue class="mx-a h-full" :value="value" :size="500" level="M" render-as="svg" foreground="hsl(248, 38%, 15%)" background="#ffffff" /></a>
</template>