<template>
    <div>
        <div class="flex justify-between items-stretch" @keydown="handleArrows">
            <input type="text" @focus="isOpen = true" placeholder="Filter" :value="searchTerm" @input="(evt) => searchTerm = evt.target.value"  class="flex-1 block text-white" />
            <button @click='isOpen = !isOpen' class="text-white">+</button>
        </div>
        <br />
            <div class="drop-wrapper bg-white rounded-lg overflow-auto" v-show='isOpen'>
                <slot />
            </div>
    </div>
</template>
  
<script lang="ts" setup>
import { provide, ref, computed, watch } from 'vue';

const options = ref([]);
const isOpen = ref(false);
const value = ref('');
const searchTerm = ref('');
const highLighted = ref('');
const highLighedIndex = ref(-1);

const registerOption = (option) => {
    options.value.push(option);
};
const selectOpton = (selectedVal: string) => {
    isOpen.value = false;
    value.value = selectedVal;
};
const selectedValue = computed(() => {
    return value.value;
})
const _searchTerm = computed(() => {
    return searchTerm.value;
})
const _highLighted = computed(() => {
    return highLighted.value;
});

const handleArrows = (evt: KeyboardEvent) => {
    if(!isOpen.value) return;
    if(evt.key === 'ArrowDown') {
        highLighedIndex.value = highLighedIndex.value >= options.value.length  - 1? options.value.length - 1 : highLighedIndex.value + 1;
        highLighted.value = options.value[highLighedIndex.value];
    }
    if(evt.key === 'ArrowUp') {
        highLighedIndex.value = highLighedIndex.value === 0 ? 0 : highLighedIndex.value - 1;
        highLighted.value = options.value[highLighedIndex.value];
    }
    if(evt.key === 'Enter' && highLighedIndex.value >= 0) {
        value.value = highLighted.value;
        isOpen.value = false;
    }

}
watch(isOpen, (val) => {
    if(!val) {
        searchTerm.value = '';
        highLighedIndex.value = -1;
        highLighted.value = '';
    }
})
provide('options', {
    registerOption,
    selectOpton,
    selectedValue,
    highLighted: _highLighted,
    searchTerm: _searchTerm
});

</script>
  
<style>
.drop-wrapper {
    display: flex;
    flex-direction: column;
    max-height: 160px;
}
</style>