<template>
  <q-page padding>
    <p class="text-h6">Page - 1</p>
    <div style="max-width: 420px; margin: auto">
      <q-form class="q-gutter-md">
        <SelectInput
          label="Select Province"
          :options="provinceOptions"
          @onChange="onProvinceChange"
        />
        <SelectInput
          ref="citySelector"
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
import SelectInput, { SelectInputMethod } from 'src/components/SelectInput.vue';
import { City } from 'src/models/city';
import { Province } from 'src/models/province';
import { onMounted, ref } from 'vue';

const selectedProvince = ref('');
const selectedCity = ref('');
const provinceOptions = ref<Province[]>([]);
const cityOptions = ref<City[]>([]);
const citySelector = ref(null);

function onProvinceChange(value: Province) {
  selectedProvince.value = value.label;
  loadCities(value.id);
  (citySelector.value as unknown as SelectInputMethod)?.resetSelectedOption();
}

function onCityChange(value: City) {
  selectedCity.value = value.label;
}

async function loadProvinces() {
  const res = await api.get('provinces');
  provinceOptions.value = res.data;
}

async function loadCities(id: number) {
  const res = await api.get('cities');
  const cities = res.data?.filter((city: City) => city.provinceId == id);
  cityOptions.value = cities;
}

onMounted(() => {
  loadProvinces();
});
</script>
