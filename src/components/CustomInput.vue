<script setup>
defineProps(['modelValue']);
defineEmits(['update:modelValue']);
</script>
<template class="input-com">
  输入你的习惯：
  <input
    :value="modelValue"
    @input="$emit('update:modelValue', $event.target.value)"
  />
</template>
<style scoped>
.input-com {
  border: 1px solid green;
  margin-top: 10px;
  color: #fff;
}

</style>
