<script setup lang="ts">
import VirtualizedSelect from "./components/VirtualizedSelect.vue";
import { ref } from "vue";
import { nanoid } from "nanoid";
import NormalSelect from "./components/NormalSelect.vue";

const items = ref(
  Array.from({ length: 50 }, (_, i) => ({
    id: nanoid(),
    label: `Item ${i}`,
  }))
);

const selected = ref<{ id: string; label: string }>();
const selected2 = ref<{ id: string; label: string }>();
</script>

<template>
  <div class="flex flex-wrap">
    <div class="space-y-4 w-1/2 flex justify-center">
      <div class="text-sm">selected: {{ selected?.label }}</div>
      <VirtualizedSelect :items="items" v-model="selected" />
    </div>
    <div class="space-y-4 w-1/2 flex justify-center">
      <div class="text-sm">selected: {{ selected2?.label }}</div>
      <NormalSelect :items="items" v-model="selected2" />
    </div>
  </div>
</template>
