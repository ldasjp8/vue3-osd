<script setup lang="ts">
import Osd from '@/components/Osd.vue'
import { onMounted, ref } from 'vue'
import { useRoute } from 'vue-router'

const page = ref<number>(1)
const manifest = ref<string>("")

const route = useRoute()

const updatePage = (value: number) => {
  page.value = value
}

onMounted(() => {
  manifest.value = route.query.manifest ? String(route.query.manifest) : "https://www.dl.ndl.go.jp/api/iiif/3437686/manifest.json"
})
</script>

<template>
  <p>Page: {{ page }}</p>
  <Osd
    v-if="manifest"
    @page="updatePage"
    :manifest="manifest"
  />
  <p>
    <ul>
      <li>This demo application processes manifest files for which the IIIF Presentation API v.2 and Image API are enabled.</li>
      <li>本デモアプリケーションでは、IIIF Presentation API v.2 および Image API が有効なマニフェストファイルが処理対象です。</li>
    </ul></p>
</template>
