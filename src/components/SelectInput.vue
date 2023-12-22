<template>
  <q-select filled v-model="selectedOption" :label="label" :options="options" />
</template>

<script setup lang="ts">
import { ref, watch } from 'vue';

type SelectProps = {
  label: string;
  options?: object[];
};

export type SelectInputMethod = {
  resetSelectedOption: () => void;
};

const selectedOption = ref(undefined);

withDefaults(defineProps<SelectProps>(), {
  label: '',
  options: undefined,
});

const emits = defineEmits(['onChange']);
watch(selectedOption, () => {
  emits('onChange', selectedOption.value ?? '');
});

function resetSelectedOption() {
  selectedOption.value = undefined;
}

defineExpose({
  resetSelectedOption,
});
</script>
