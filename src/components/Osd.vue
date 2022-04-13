<script setup lang="ts">
import { onMounted, ref } from 'vue'
import OpenSeadragon from 'openseadragon'
import axios from 'axios'

const title = ref<string>("")
const attribution = ref<string>("")

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
  (action: string, page: number): void
}>()

onMounted(async () => {
  const tileSources: string[] = []

  const { data } = await axios.get(props.manifest)
  title.value = data.label
  attribution.value = data.attribution

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
  <p>Title: <b>{{title}}</b></p>
  <p>Attribution: <b>{{attribution}}</b></p>
  <div
    id="openseadragon"
    :style="`height: ${height}px; width: ${width}; background-color: black;`"
  ></div>
</template>
