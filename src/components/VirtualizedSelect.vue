<script setup lang="ts">
import { computed } from "vue";
import {
  SelectContent,
  SelectItem,
  SelectPortal,
  SelectRoot,
  SelectTrigger,
  SelectValue,
} from "radix-vue";

import { useVirtualList } from "@vueuse/core";

const props = withDefaults(
  defineProps<{
    items: { id: string; label: string }[];
    modelValue?: { id: string; label: string };
  }>(),
  {
    modelValue: undefined,
  }
);

const emit = defineEmits<{
  "update:modelValue": [{ id: string; label: string } | undefined];
}>();

const selected = computed({
  get: () => props.modelValue?.id,
  set: (value) =>
    emit(
      "update:modelValue",
      props.items.find((i) => i.id === value)
    ),
});

const { list, containerProps, wrapperProps } = useVirtualList(props.items, {
  itemHeight: 32,
});
</script>

<template>
  <SelectRoot v-model="selected">
    <SelectTrigger class="py-1 px-3 bg-gray-200">
      <SelectValue placeholder="Virtualized Select...">
        {{ props.modelValue?.label ?? "Virtualized Select..." }}
      </SelectValue>
    </SelectTrigger>

    <SelectPortal>
      <SelectContent :side-offset="5" as-child position="popper">
        <div
          class="bg-white shadow-lg border border-gray-200"
          v-bind="containerProps"
          style="max-height: 300px"
        >
          <div v-bind="wrapperProps">
            <SelectItem
              style="height: 32px"
              v-for="item in list"
              :key="item.index"
              :value="item.data.id"
              class="py-1 px-3 data-[highlighted]:bg-gray-200"
              >{{ item.data.label }}</SelectItem
            >
          </div>
        </div>
      </SelectContent>
    </SelectPortal>
  </SelectRoot>
</template>
