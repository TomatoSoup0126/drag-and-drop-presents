<script setup lang="ts">
import ChristmasPresent from './components/ChristmasPresent.vue'
import ChristmasTree from './components/ChristmasTree.vue'
import { ref } from 'vue'

const presents = ref(['small-red-gift', 'blue-gift', 'tall-red-gift'])
const underTheTree = ref([])

const startDrag = (evt, item) => {
  evt.dataTransfer.dropEffect = 'move'
  evt.dataTransfer.effectAllowed = 'move'
  evt.dataTransfer.setData('item', item)
}

const onDrop = evt => {
  const item:string = evt.dataTransfer.getData('item')
  presents.value = presents.value.filter((present) => present !== item)
  underTheTree.value.push(item)
}
</script>

<template>
  <div class="flex flex-col items-center mt-24 min-h-screen w-full">
    <h1 class="mt-8 text-xl font-bold">Drag the presents under the tree!</h1>
    <div
      @drop="onDrop($event)"
      @dragover.prevent
      @dragenter.prevent
    >
      <ChristmasTree :presents="underTheTree" class="mt-16" />
    </div>
    <div class="pt-32 mt-32 bg-gray-100 w-full justify-center flex-1">
      <div
        class="flex items-end justify-center"
        v-auto-animate
      >
        <ChristmasPresent
          v-for="p in presents"
          :key="p"
          :name="p"
          :draggable="true"
          @dragstart="startDrag($event, p)"
        />
      </div>
    </div>
  </div>
</template>
