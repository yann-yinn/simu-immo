<template>
  <div>
    <h2 class="text-xl font-bold mb-4">Informations principales du bien</h2>
    <form class="space-y-4">
      <!-- Type de bien -->
      <div>
        <label class="block mb-1 font-medium">Type de bien</label>
        <select v-model="local.type" @change="emitChange" class="w-full border rounded px-3 py-2">
          <option disabled value="">Sélectionnez</option>
          <option value="maison">Maison</option>
          <option value="appartement">Appartement</option>
        </select>
      </div>
      <!-- Surface -->
      <div>
        <label class="block mb-1 font-medium">Surface (m² loi Carrez)</label>
        <input
          type="number"
          min="0"
          v-model.number="local.surface"
          @input="emitChange"
          class="w-full border rounded px-3 py-2"
          placeholder="Ex: 75"
        />
      </div>
      <!-- Année de construction -->
      <div>
        <label class="block mb-1 font-medium">Année de construction</label>
        <select v-model="local.annee" @change="emitChange" class="w-full border rounded px-3 py-2">
          <option disabled value="">Sélectionnez</option>
          <option value="avant_1850">Avant 1850</option>
          <option value="1850_1913">1850 - 1913</option>
          <option value="1914_1947">1914 - 1947</option>
          <option value="1948_1969">1948 - 1969</option>
          <option value="1970_1980">1970 - 1980</option>
          <option value="1981_1991">1981 - 1991</option>
          <option value="1992_2000">1992 - 2000</option>
          <option value="2001_2010">2001 - 2010</option>
          <option value="apres_2011">Après 2011</option>
          <option value="unknown">Ne sais pas</option>
        </select>
      </div>
      <!-- Diagnostic -->
      <div>
        <label class="block mb-1 font-medium">Diagnostic énergétique</label>
        <select
          v-model="local.diagnostic"
          @change="emitChange"
          class="w-full border rounded px-3 py-2"
        >
          <option disabled value="">Sélectionnez</option>
          <option value="A">A</option>
          <option value="B">B</option>
          <option value="C">C</option>
          <option value="D">D</option>
          <option value="E">E</option>
          <option value="F">F</option>
          <option value="G">G</option>
          <option value="unknown">Je ne sais pas</option>
        </select>
      </div>
      <!-- Etat du bien -->
      <div>
        <label class="block mb-1 font-medium">État du bien</label>
        <select v-model="local.etat" @change="emitChange" class="w-full border rounded px-3 py-2">
          <option disabled value="">Sélectionnez</option>
          <option value="standard">Standard</option>
          <option value="refait">Refait à neuf</option>
          <option value="rafraichissement">Rafraichissement nécessaire</option>
          <option value="travaux">Travaux importants</option>
        </select>
      </div>
    </form>
  </div>
</template>

<script setup lang="ts">
import { defineProps, defineEmits, watch, ref } from 'vue'

type TypeBien = {
  type: string
  surface: number | null
  annee: string
  diagnostic: string
  etat: string
}

const props = defineProps<{ modelValue: Partial<TypeBien> }>()
const emit = defineEmits(['update:modelValue'])

const local = ref<Partial<TypeBien>>({
  type: props.modelValue.type || '',
  surface: props.modelValue.surface || null,
  annee: props.modelValue.annee || '',
  diagnostic: props.modelValue.diagnostic || '',
  etat: props.modelValue.etat || '',
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
