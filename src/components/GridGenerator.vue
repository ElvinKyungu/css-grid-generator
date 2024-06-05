<template>
  <div class="p-6 mx-20">
    <h1 class="my-10 text-2xl">Petit générateur de css grid</h1>
    <div class="flex gap-4 mb-4">
      <div>
        <label for="rows" class="block text-sm font-medium text-gray-700">Nombre de lignes</label>
        <input type="number" v-model="rows" id="rows" class="mt-1 block w-full p-2 border border-gray-300 rounded-md" />
      </div>
      <div>
        <label for="columns" class="block text-sm font-medium text-gray-700">Nombre de colonnes</label>
        <input type="number" v-model="columns" id="columns" class="mt-1 block w-full p-2 border border-gray-300 rounded-md" />
      </div>
      <div>
        <label for="columnGap" class="block text-sm font-medium text-gray-700">Column Gap (px)</label>
        <input type="number" v-model="columnGap" id="columnGap" class="mt-1 block w-full p-2 border border-gray-300 rounded-md" />
      </div>
      <div>
        <label for="rowGap" class="block text-sm font-medium text-gray-700">Row Gap (px)</label>
        <input type="number" v-model="rowGap" id="rowGap" class="mt-1 block w-full p-2 border border-gray-300 rounded-md" />
      </div>
    </div>
    <button @click="generateGrid" class="px-4 py-2 bg-blue-500 text-white rounded-md">Générer</button>

    <div 
      v-if="grid" 
      :style="gridStyle" 
      class="mt-6 border"
    >
      <div 
        v-for="(cell, index) in totalCells" 
        :key="index" 
        class="border border-black w-full h-28 bg-slate-100"
      >
      </div>
    </div>

    <button @click="toggleShowCode" class="mt-4 px-4 py-2 bg-gray-500 text-white rounded-md">Show me the code</button>
    <pre v-if="showCode" class="mt-4 p-4 bg-gray-100 border rounded-md">{{ code }}</pre>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

const rows = ref<number>(0)
const columns = ref<number>(0)
const columnGap = ref<number>(0)
const rowGap = ref<number>(0)
const grid = ref<number[][] | null>(null)
const showCode = ref<boolean>(false)

const generateGrid = () => {
  grid.value = Array.from({ length: rows.value }, () => Array(columns.value).fill(0))
}

const totalCells = computed(() => {
  return rows.value * columns.value;
})

const gridStyle = computed(() => ({
  display: 'grid',
  gridTemplateRows: `repeat(${rows.value}, 1fr)`,
  gridTemplateColumns: `repeat(${columns.value}, 1fr)`,
  gap: `${rowGap.value}px ${columnGap.value}px`
}))

const toggleShowCode = () => {
  showCode.value = !showCode.value
}

const code = computed(() => {
  return `<div style="display: grid; grid-template-rows: repeat(${rows.value}, 1fr); grid-template-columns: repeat(${columns.value}, 1fr); gap: ${rowGap.value}px ${columnGap.value}px;">
  ${Array.from({ length: totalCells.value }).map(() => `<div class="border w-full h-16"></div>`).join('\n  ')}
</div>`
})
</script>

<style scoped>
input, label {
  display: block;
}
</style>
