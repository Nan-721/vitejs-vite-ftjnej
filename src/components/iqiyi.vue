<template>
  <!-- tab -->
  <div class="video-tab">
    <div
      v-for="(item, index) in tabName"
      :key="index"
      :class="['tab-item', { 'tab-active': selectTab == item }]"
      @click="selectTab = item"
    >
      {{ item }}
    </div>
  </div>
  <!-- 数据列表 -->
  <div class="list-data">
    <div v-for="item in videoList"></div>
  </div>
</template>
<script setup>
import { ref } from 'vue';
const tabName = ['综艺', '电影', '电视剧', '动漫'];
const selectTab = ref('综艺');
</script>
<style>
.video-tab {
  display: flex;
  margin-top: 20px;
}
.tab-item {
  margin-right: 20px;
}
.tab-active {
  color: red;
}
</style>
