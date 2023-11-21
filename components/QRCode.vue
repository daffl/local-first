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

// Get the location href until the last /
const href = window.location.href.substring(0, window.location.href.lastIndexOf('/'))
const hash = localStorage.getItem('automerge:repo')
const value = ref(props.value || props.page ? `${href}/${props.page}#${hash}` : `${href}#${hash}`)
</script>

<template>
  <a :href="value"><qrcode-vue class="mx-a h-full" :value="value" :size="500" level="M" render-as="svg" foreground="hsl(248, 38%, 15%)" background="#ffffff" /></a>
</template>