<script setup lang="ts">
import { computed, onMounted } from 'vue'
import OpenSeadragon from 'openseadragon'
import axios from 'axios'

const props = withDefaults(
  defineProps<{
    height?: number
    width?: string
    manifest?: string
  }>(),
  {
    height: 600,
    width: '100%',
    manifest: ''
  }
)

const emit = defineEmits<{
  (event: any, page: number): void
}>()

onMounted(async () => {
  const tileSources: any[] = []

  const { data } = await axios.get(props.manifest)

  const canvases = data.sequences[0].canvases
  for (const canvas of canvases) {
    tileSources.push(canvas.images[0].resource.service['@id'] + '/info.json')
  }

  const viewer = OpenSeadragon({
    sequenceMode: true,
    id: 'openseadragon',
    prefixUrl: "./images/",
    tileSources
  })

  viewer.addHandler('page', function (event: any) {
    emit('page', event.page + 1)
  })
})
</script>

<template>
  <div
    id="openseadragon"
    :style="`height: ${height}px; width: ${width};`"
  ></div>
</template>
