<template>
	<div :class="['drop-item', rootClasses]" @click="onSelect" v-if='isInSearchTerm' ref="el">
    {{ label }} - {{ subtext }}
  </div>
</template>
<script lang="ts" setup>
import { inject, onMounted, computed, ref, watch } from 'vue';
  
const el = ref<HTMLElement>();
const options = inject('options')

const props = defineProps({
  label: {
    type: String,
    default: undefined
  },
  subtext: {
    type: String,
    default: undefined
  },
  value: {
    type: String,
    default: undefined
  }
})
const onSelect = () => {
  options.selectOpton(props.value);
}
const isHightlighted = computed(() => {
  if(!options.highLighted.value) return false;
  return  props.value === options.highLighted.value;
})
const rootClasses = computed(() => {
  return {
    'drop-item--selected': options.selectedValue.value === props.value,
    'drop-item--focused': isHightlighted.value
  }
})

watch(isHightlighted, (val) => {
    if(val) {
        el.value?.scrollIntoView({ block: 'nearest', inline: 'start' })
    }
})

const isInSearchTerm = computed(() => {
  if(!options.searchTerm.value) return true;
  return  props.label.toLowerCase().includes(options.searchTerm.value?.toLowerCase())
})
onMounted(() => {
  options.registerOption(props.value)
})
</script>
<style>
  .drop-item {
    padding: 6px 4px;
    color: rgb(24, 23, 23);
  }
  .drop-item--focused {
    background: indigo !important;
    color: white;
  }
  .drop-item:hover {
    cursor: pointer;
    background: rgb(9, 158, 158);
    color: white;
  }
  .drop-item--selected {
    background: rgb(38, 143, 38);
    color: white;
  }
</style>