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
</script>

<template>
  <SelectRoot v-model="selected">
    <SelectTrigger class="py-1 px-3 bg-gray-200">
      <SelectValue placeholder="Normal Select...">
        {{ props.modelValue?.label ?? "Normal Select..." }}
      </SelectValue>
    </SelectTrigger>

    <SelectPortal>
      <SelectContent :side-offset="5" as-child position="popper">
        <div
          class="bg-white shadow-lg border border-gray-200 overflow-auto"
          style="max-height: 300px"
        >
          <SelectItem
            v-for="item in items"
            :key="item.id"
            :value="item.id"
            class="py-1 px-3 data-[highlighted]:bg-gray-200"
            >{{ item.label }}</SelectItem
          >
        </div>
      </SelectContent>
    </SelectPortal>
  </SelectRoot>
</template>
