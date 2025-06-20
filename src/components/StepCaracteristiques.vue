<template>
  <div>
    <h2 class="text-xl font-bold mb-4">Caractéristiques du bien (facultatif)</h2>
    <form class="space-y-2">
      <!-- @ts-ignore: carac est de type { value: keyof Caracteristiques, label: string } -->
      <div v-for="carac in caracteristiquesList" :key="carac.value" class="flex items-center">
        <input
          type="checkbox"
          :id="carac.value"
          :value="carac.value"
          v-model="local[carac.value as keyof Caracteristiques]"
          @change="emitChange"
          class="mr-2"
        />
        <label :for="carac.value">{{ carac.label }}</label>
      </div>
    </form>
  </div>
</template>

<script setup lang="ts">
import { defineProps, defineEmits, watch, ref } from 'vue'

type Caracteristiques = {
  ascenseur?: boolean
  cave?: boolean
  balcon?: boolean
  terrasse?: boolean
  parking?: boolean
  vue?: boolean
  calme?: boolean
  ravalement?: boolean
}

const caracteristiquesList = [
  { value: 'ascenseur', label: 'Ascenseur' },
  { value: 'cave', label: 'Cave' },
  { value: 'balcon', label: 'Balcon' },
  { value: 'terrasse', label: 'Terrasse' },
  { value: 'parking', label: 'Parking' },
  { value: 'vue', label: 'Vue exceptionnelle' },
  { value: 'calme', label: 'Logement calme' },
  { value: 'ravalement', label: 'Ravalement de façade récent' },
]

const props = defineProps<{ modelValue: Partial<Caracteristiques> }>()
const emit = defineEmits(['update:modelValue'])

const local = ref<Partial<Caracteristiques>>({
  ascenseur: props.modelValue.ascenseur || false,
  cave: props.modelValue.cave || false,
  balcon: props.modelValue.balcon || false,
  terrasse: props.modelValue.terrasse || false,
  parking: props.modelValue.parking || false,
  vue: props.modelValue.vue || false,
  calme: props.modelValue.calme || false,
  ravalement: props.modelValue.ravalement || false,
})

watch(
  () => props.modelValue,
  (nv) => {
    local.value = { ...local.value, ...nv }
  },
)

function emitChange() {
  emit('update:modelValue', { ...local.value })
}
</script>
