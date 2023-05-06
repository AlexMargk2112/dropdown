<script setup lang="ts">
import Dropdown from './components/Dropdown.vue';
import DropdownItem from './components/DropdownItem.vue';
</script>

<template>
  <Dropdown>
  	<DropdownItem value="ab" label="ABC" subtext="Dummy as fuck"/>
    <DropdownItem value="ac" label="ACD" subtext="Dummy as fuck two times"/>
    <DropdownItem value="dc" label="FDL" subtext="Dummy as fuck three times"/>
    <DropdownItem value="lc" label="LCL" subtext="WTF"/>
  </Dropdown>
</template>
