<template>
  <div class="app">
    <header class="header">
      <h1>Vue 文档预览 Demo</h1>
      <p>基于 @vue-office 系列，支持 Word / Excel / PDF / PPTX</p>
    </header>

    <nav class="tabs">
      <button
        v-for="(item, key) in previewers"
        :key="key"
        class="tab"
        :class="{ active: activeType === key }"
        @click="switchType(key)"
      >
        {{ item.label }}
      </button>
    </nav>

    <section class="toolbar">
      <label class="file-btn">
        选择文件
        <input
          type="file"
          :accept="previewers[activeType].accept"
          @change="onFileChange"
        />
      </label>
      <input
        v-model="url"
        class="url-input"
        type="text"
        placeholder="粘贴文件 URL（http/https），回车预览"
        @keyup.enter="loadUrl"
      />
      <button class="btn" @click="loadUrl">预览 URL</button>
      <button class="btn btn-plain" @click="reset">清空</button>
    </section>

    <main class="preview">
      <p v-if="!src" class="placeholder">
        请选择本地文件或输入一个 URL 开始预览
      </p>
      <component
        :is="previewers[activeType].component"
        v-else
        :src="src"
        class="previewer"
      />
    </main>
  </div>
</template>

<script setup>
import { ref, markRaw } from 'vue'
import VueOfficeDocx from '@vue-office/docx'
import VueOfficeExcel from '@vue-office/excel'
import VueOfficePdf from '@vue-office/pdf'
import VueOfficePptx from '@vue-office/pptx'

import '@vue-office/docx/lib/index.css'
import '@vue-office/excel/lib/index.css'
import '@vue-office/pdf/lib/index.css'
import '@vue-office/pptx/lib/index.css'

const previewers = {
  docx: {
    label: 'Word',
    component: markRaw(VueOfficeDocx),
    accept: '.docx',
  },
  excel: {
    label: 'Excel',
    component: markRaw(VueOfficeExcel),
    accept: '.xlsx,.xls',
  },
  pdf: {
    label: 'PDF',
    component: markRaw(VueOfficePdf),
    accept: '.pdf',
  },
  pptx: {
    label: 'PPTX',
    component: markRaw(VueOfficePptx),
    accept: '.pptx',
  },
}

const activeType = ref('docx')
const src = ref('')
const url = ref('')

function switchType(key) {
  activeType.value = key
  src.value = ''
  url.value = ''
}

function loadUrl() {
  const value = url.value.trim()
  if (value) {
    src.value = value
  }
}

function onFileChange(event) {
  const file = event.target.files && event.target.files[0]
  if (!file) return
  const reader = new FileReader()
  reader.onload = () => {
    src.value = reader.result
  }
  reader.readAsArrayBuffer(file)
  event.target.value = ''
}

function reset() {
  src.value = ''
  url.value = ''
}
</script>

<style scoped>
.app {
  max-width: 1100px;
  margin: 0 auto;
  padding: 24px;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  color: #1f2937;
}

.header h1 {
  margin: 0;
  font-size: 24px;
}

.header p {
  margin: 6px 0 20px;
  color: #6b7280;
}

.tabs {
  display: flex;
  gap: 8px;
  border-bottom: 1px solid #e5e7eb;
  padding-bottom: 12px;
}

.tab {
  border: 1px solid #e5e7eb;
  background: #fff;
  color: #374151;
  padding: 8px 18px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 14px;
}

.tab.active {
  background: #2563eb;
  border-color: #2563eb;
  color: #fff;
}

.toolbar {
  display: flex;
  gap: 10px;
  align-items: center;
  margin: 16px 0;
  flex-wrap: wrap;
}

.file-btn {
  position: relative;
  overflow: hidden;
  background: #2563eb;
  color: #fff;
  padding: 8px 16px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 14px;
}

.file-btn input {
  position: absolute;
  inset: 0;
  opacity: 0;
  cursor: pointer;
}

.url-input {
  flex: 1;
  min-width: 240px;
  padding: 8px 12px;
  border: 1px solid #d1d5db;
  border-radius: 8px;
  font-size: 14px;
}

.btn {
  background: #2563eb;
  color: #fff;
  border: none;
  padding: 8px 16px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 14px;
}

.btn-plain {
  background: #f3f4f6;
  color: #374151;
}

.preview {
  border: 1px solid #e5e7eb;
  border-radius: 12px;
  height: 620px;
  overflow: auto;
}

.placeholder {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
  margin: 0;
  color: #9ca3af;
}

.previewer {
  width: 100%;
  min-height: 100%;
}
</style>
