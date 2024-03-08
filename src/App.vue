<script setup lang="ts">

import { VuePDF, usePDF } from '@tato30/vue-pdf'
import '@tato30/vue-pdf/style.css'

const pdfURL = 'https://arxiv.org/pdf/1506.02640.pdf'
const { pdf, pages } = usePDF(pdfURL)

</script>

<template>
<div class="flex w-screen h-screen items-stretch">
  <div class="flex-1 overflow-y-auto">
    <div v-for="page in pages" :key="page">
      <VuePDF :pdf="pdf" :page="page" text-layer annotation-layer fit-parent />
    </div>
  </div>
  <hr class="border-r border-r-slate-400 h-full">
  <div class="flex-1">
    asd
  </div>
</div>
</template>
