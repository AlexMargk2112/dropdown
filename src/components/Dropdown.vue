<template>
    <div>
        <div class="flex justify-between items-stretch">
            <input @focus="isOpen = true" placeholder="Filter" v-model="searchTerm" class="flex-1 block text-white" />
            <button @click='isOpen = !isOpen' class="text-white">+</button>
        </div>
        <br />
            <div class="drop-wrapper bg-white rounded-lg overflow-auto" v-show='isOpen'>
                <slot />
            </div>
    </div>
</template>
  
<script lang="ts" setup>
import { provide, ref, computed } from 'vue';

const options = ref([]);
const isOpen = ref(false);
const value = ref('ab');
const searchTerm = ref('');

const registerOption = (option) => {
    options.value.push(option);
};
const selectOpton = (selectedVal) => {
    isOpen.value = false;
    searchTerm.value = '';
    value.value = selectedVal;
};
const selectedValue = computed(() => {
    return value.value;
})
const _searchTerm = computed(() => {
    return searchTerm.value;
})
provide('options', {
    registerOption,
    selectOpton,
    selectedValue,
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