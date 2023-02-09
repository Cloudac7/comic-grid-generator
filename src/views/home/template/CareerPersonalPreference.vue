<template>
  <div class="w-full bg-white p-6" un-flex="col" ref="picAreaRef">
    <h2 un-text="12">漫画生涯个人喜好表</h2>
    <div un-flex="shrink-0" un-grid="~ cols-6" class="gap-2">
      <GameCard :item="i" v-for="i in gameList" :key="i.label"></GameCard>
    </div>
  </div>
  <div class="mt-4 space-x-4">
    <button @click="exportData">导出数据</button>
    <button @click="generateAPicture">生成图片</button>
  </div>
</template>

<script setup lang="ts">
import GameCard from '@/components/GameCard.vue';
import type { GameCardItem } from '@/types';
import { saveAs } from 'file-saver';
import * as htmlToImage from 'html-to-image';
import { reactive, ref } from 'vue';

const gameList = reactive<GameCardItem[]>([
    '入坑作',
    '最喜欢',
    '看最多次',
    '最想安利',
    '最佳剧情',
    '最爱画风',
    '最优人设',
    '最强分镜',
    '最惊喜',
    '最治愈',
    '最感动',
    '最虐心',
    '最被低估',
    '有生之年',
    '最离谱',
    '最过誉',
    '最遗憾',
    '最讨厌'
].map(label => ({
  label,
  content: ''
})))

const picAreaRef = ref<HTMLDivElement | null>(null)

/**
 * 导出数据
 */
function exportData() {
  const data = {
    version: 1,
    list: gameList
  }
  saveAs(new Blob([JSON.stringify(data)], {type: "text/plain;charset=utf-8"}), '漫画生涯个人喜好表.json')
}

/**
 * 生成图片
 */
function generateAPicture() {
  if (picAreaRef.value) {
    htmlToImage.toBlob(picAreaRef.value)
        .then(blob => {
          if (blob) {
            saveAs(blob, '漫画生涯个人喜好表.png');
          }
        })
        .catch(function (error) {
          console.error('生成失败!', error);
        });
  }
}
</script>

<style scoped>

</style>
