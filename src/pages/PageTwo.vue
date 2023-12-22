<template>
  <q-page padding>
    <p class="text-h6">Page - 2</p>
    <div style="max-width: 420px; margin: auto">
      <q-form class="q-gutter-md">
        <SelectInput
          label="Select City"
          :options="cityOptions"
          @onChange="onCityChange"
        />
      </q-form>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import { api } from 'src/boot/axios';
import SelectInput from 'src/components/SelectInput.vue';
import { City } from 'src/models/city';
import { onMounted, ref } from 'vue';

const selectedCity = ref('');
const cityOptions = ref<City[]>([]);

function onCityChange(value: City) {
  selectedCity.value = value.label;
}

async function loadCities() {
  const res = await api.get('cities');
  cityOptions.value = res.data;
}

onMounted(() => {
  loadCities();
});
</script>
