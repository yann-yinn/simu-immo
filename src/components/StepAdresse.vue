<template>
  <div>
    <h2 class="text-xl font-bold mb-4">Entrez l'adresse de votre bien</h2>
    <div class="relative">
      <input
        type="text"
        class="w-full border rounded px-3 py-2"
        placeholder="Adresse du bien"
        :value="search"
        @input="onInput"
        @focus="showSuggestions = true"
        @blur="onBlur"
        autocomplete="off"
      />
      <ul
        v-if="showSuggestions && suggestions.length"
        class="absolute z-10 bg-white border w-full mt-1 rounded shadow max-h-56 overflow-auto"
      >
        <li
          v-for="(item, i) in suggestions"
          :key="i"
          @mousedown.prevent="selectSuggestion(item)"
          class="px-3 py-2 cursor-pointer hover:bg-blue-100"
        >
          {{ item.label }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup lang="ts">
import { defineProps, defineEmits, ref, watch } from 'vue'

interface AdresseSuggestion {
  label: string
  city?: string
  postcode?: string
  lat?: number
  lon?: number
  [key: string]: any
}

const props = defineProps<{ modelValue: AdresseSuggestion | string }>()
const emit = defineEmits(['update:modelValue'])

const search = ref(
  typeof props.modelValue === 'string' ? props.modelValue : props.modelValue?.label || '',
)
const suggestions = ref<AdresseSuggestion[]>([])
const showSuggestions = ref(false)
let debounceTimeout: ReturnType<typeof setTimeout> | null = null

watch(
  () => props.modelValue,
  (nv) => {
    if (typeof nv === 'string') search.value = nv
    else if (nv && typeof nv === 'object') search.value = nv.label
  },
)

function onInput(e: Event) {
  search.value = (e.target as HTMLInputElement).value
  emit('update:modelValue', search.value)
  fetchSuggestions()
}

function fetchSuggestions() {
  if (debounceTimeout) clearTimeout(debounceTimeout)
  if (!search.value || search.value.length < 3) {
    suggestions.value = []
    return
  }
  debounceTimeout = setTimeout(async () => {
    const resp = await fetch(
      `https://api-adresse.data.gouv.fr/search/?q=${encodeURIComponent(search.value)}&limit=5`,
    )
    const data = await resp.json()
    suggestions.value = data.features.map(
      (f: { properties: Record<string, any>; geometry: { coordinates: [number, number] } }) => ({
        label: f.properties.label,
        city: f.properties.city,
        postcode: f.properties.postcode,
        lat: f.geometry.coordinates[1],
        lon: f.geometry.coordinates[0],
        ...f.properties,
      }),
    )
  }, 250)
}

function selectSuggestion(item: AdresseSuggestion) {
  search.value = item.label
  emit('update:modelValue', item)
  showSuggestions.value = false
}

function onBlur() {
  setTimeout(() => {
    showSuggestions.value = false
  }, 150)
}
</script>
