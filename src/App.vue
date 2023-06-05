<script setup>
import { ref, reactive, onMounted, nextTick, computed, watch } from 'vue';

import HelloWorld from './components/HelloWorld.vue';
import CustomInput from './components/CustomInput.vue';
import myModal from './components/myModal.vue';
import iqiyi from './components/iqiyi.vue';
const searchText = ref('hello');

const currentTab = ref('iqiyi');
const tabs = {
  iqiyi,
  HelloWorld,
  myModal,
  CustomInput,
};
</script>

<template>
  <button
    v-for="(_, tab) in tabs"
    :key="tab"
    :class="['tab-button', { active: currentTab == tab }]"
    @click="currentTab = tab"
  >
    {{ tab }}
  </button>
  <component :is="tabs[currentTab]"></component>
  <br />

</template>

<style scoped>
.tab-button {
  padding: 6px 10px;
  border-top-left-radius: 3px;
  border-top-right-radius: 3px;
  border: 1px solid #ccc;
  cursor: pointer;
  background: #f0f0f0;
  margin-bottom: -1px;
  margin-right: -1px;
}
.tab-button:hover {
  background: #e0e0e0;
}
.tab-button.active {
  background: #e0e0e0;
}
.tab {
  border: 1px solid #ccc;
  padding: 10px;
}

</style>
