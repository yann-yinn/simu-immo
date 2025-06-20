<template>
  <div>
    <h2 class="text-xl font-bold mb-4">Précisions sur le bien</h2>
    <form class="space-y-4">
      <!-- Nombre de pièces -->
      <div>
        <label class="block mb-1 font-medium">Nombre de pièces</label>
        <input
          type="number"
          min="0"
          v-model.number="local.pieces"
          @input="emitChange"
          class="w-full border rounded px-3 py-2"
          placeholder="Ex: 4"
        />
      </div>
      <!-- Nombre de chambres -->
      <div>
        <label class="block mb-1 font-medium">Nombre de chambres</label>
        <input
          type="number"
          min="0"
          v-model.number="local.chambres"
          @input="emitChange"
          class="w-full border rounded px-3 py-2"
          placeholder="Ex: 2"
        />
      </div>
      <!-- Nombre de salles de bain -->
      <div>
        <label class="block mb-1 font-medium">Nombre de salles de bain</label>
        <input
          type="number"
          min="0"
          v-model.number="local.sdb"
          @input="emitChange"
          class="w-full border rounded px-3 py-2"
          placeholder="Ex: 1"
        />
      </div>
      <!-- Nombre d'étages dans l'immeuble -->
      <div>
        <label class="block mb-1 font-medium">Nombre d'étages dans l'immeuble</label>
        <input
          type="number"
          min="0"
          v-model.number="local.etagesImmeuble"
          @input="emitChange"
          class="w-full border rounded px-3 py-2"
          placeholder="Ex: 5"
        />
      </div>
      <!-- Etage de ce bien -->
      <div>
        <label class="block mb-1 font-medium">Étage de ce bien</label>
        <input
          type="number"
          min="0"
          v-model.number="local.etageBien"
          @input="emitChange"
          class="w-full border rounded px-3 py-2"
          placeholder="Ex: 2"
        />
      </div>
    </form>
  </div>
</template>

<script setup lang="ts">
import { defineProps, defineEmits, watch, ref } from 'vue'

type PrecisionsBien = {
  pieces: number | null
  chambres: number | null
  sdb: number | null
  etagesImmeuble: number | null
  etageBien: number | null
}

const props = defineProps<{ modelValue: Partial<PrecisionsBien> }>()
const emit = defineEmits(['update:modelValue'])

const local = ref<Partial<PrecisionsBien>>({
  pieces: props.modelValue.pieces || null,
  chambres: props.modelValue.chambres || null,
  sdb: props.modelValue.sdb || null,
  etagesImmeuble: props.modelValue.etagesImmeuble || null,
  etageBien: props.modelValue.etageBien || null,
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
