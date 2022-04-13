<script setup lang="ts">
import Osd from '@/components/Osd.vue'
import { ref } from 'vue'

let page = ref<number>(1)

const updatePage = (value: number) => {
  page.value = value
}
</script>

<template>
  {{ page }}
  <Osd
    @page="updatePage"
    manifest="https://www.dl.ndl.go.jp/api/iiif/8929985/manifest.json"
  />
</template>
