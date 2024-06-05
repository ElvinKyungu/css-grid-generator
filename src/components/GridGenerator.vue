<template>
  <div class="p-6">
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

    <div v-if="grid" class="mt-6">
      <div class="flex">
        <div v-for="(column, index) in columns" :key="'col' + index" class="mr-2">
          <label :for="'columnSize' + index" class="block text-sm font-medium text-gray-700">Colonne {{ index + 1 }}</label>
          <input v-model="columnSizes[index]" :id="'columnSize' + index" class="mt-1 block w-full p-2 border border-gray-300 rounded-md" />
        </div>
      </div>

      <!-- <div v-for="(row, index) in rows" :key="'row' + index" class="mt-4">
        <label :for="'rowSize' + index" class="block text-sm font-medium text-gray-700">Ligne {{ index + 1 }}</label>
        <input v-model="rowSizes[index]" :id="'rowSize' + index" class="mt-1 block w-full p-2 border border-gray-300 rounded-md" />
      </div> -->

      <div :style="gridStyle" class="mt-6 border">
        <div v-for="(cell, index) in totalCells" :key="index" class="border w-full h-16"></div>
      </div>

      <button @click="toggleShowCode" class="mt-4 px-4 py-2 bg-gray-500 text-white rounded-md">Show me the code</button>
      <pre v-if="showCode" class="mt-4 p-4 bg-gray-100 border rounded-md">
        <code class="language-html">{{ code }}</code>
      </pre>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, watch } from 'vue'
import Prism from 'prismjs'
import 'prismjs/themes/prism.css'

const rows = ref<number>(0)
const columns = ref<number>(0)
const columnGap = ref<number>(0)
const rowGap = ref<number>(0)
const grid = ref<number[][] | null>(null)
const showCode = ref<boolean>(false)

const columnSizes = ref<string[]>([])
const rowSizes = ref<string[]>([])

watch([rows, columns], () => {
  columnSizes.value = Array(columns.value).fill('1fr')
  rowSizes.value = Array(rows.value).fill('1fr')
})

const generateGrid = () => {
  grid.value = Array.from({ length: rows.value }, () => Array(columns.value).fill(0))
}

const totalCells = computed(() => {
  return rows.value * columns.value;
})

const gridStyle = computed(() => ({
  display: 'grid',
  gridTemplateRows: rowSizes.value.join(' '),
  gridTemplateColumns: columnSizes.value.join(' '),
  gap: `${rowGap.value}px ${columnGap.value}px`
}))

const toggleShowCode = () => {
  showCode.value = !showCode.value
  if (showCode.value) {
    Prism.highlightAll()
  }
}

const code = computed(() => {
  return `<div style="display: grid; grid-template-rows: ${rowSizes.value.join(' ')}; grid-template-columns: ${columnSizes.value.join(' ')}; gap: ${rowGap.value}px ${columnGap.value}px;">
  ${Array.from({ length: totalCells.value }).map(() => `<div class="border w-full h-16"></div>`).join('\n  ')}
</div>`
})
</script>

<style scoped>
input, label {
  display: block;
}
</style>
