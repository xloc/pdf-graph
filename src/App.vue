<script setup lang="ts">
import { computed, ref } from 'vue';
import { useMagicKeys, whenever } from '@vueuse/core'
import { VuePDF, usePDF } from '@tato30/vue-pdf'
import '@tato30/vue-pdf/style.css'


const pdfURL = 'yolo.pdf'
const { pdf, pages } = usePDF(pdfURL)


interface Node {
  text: string
  selection: Selection
}
const nodes = ref<Node[]>([]);
const hasNodes = computed(() => nodes.value.length > 0)


const keys = useMagicKeys()
whenever(keys.n, () => {
  const selection = document.getSelection();
  if (!selection) return;

  nodes.value.push({
    text: selection.toString(),
    selection,
  })
})
whenever(keys.backspace, () => {
  nodes.value.pop()
})
whenever(keys.c, () => {
  nodes.value = []
})
</script>

<template>
  <div class="flex w-screen h-screen items-stretch">
    <div class="flex-1 overflow-y-auto">
      <div v-for="page in pages" :key="page">
        <VuePDF :pdf="pdf" :page="page" text-layer annotation-layer fit-parent />
      </div>
    </div>
    <hr class="border-r border-r-slate-400 h-full">
    <div class="flex-1" :class="{ 'flex justify-center items-center': !hasNodes }">
      <div v-if="!hasNodes" class="text-gray-400">
        Empty
      </div>
      <div v-else>
        <div v-for="node in nodes" :key="node.text">
          <div class="p-2 m-2 border rounded-lg">
            {{ node.text }}</div>
        </div>
      </div>
    </div>
  </div>
</template>
