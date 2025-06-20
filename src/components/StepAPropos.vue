<template>
  <div>
    <h2 class="text-xl font-bold mb-4">À propos de vous</h2>
    <form class="space-y-4">
      <!-- Nom -->
      <div>
        <label class="block mb-1 font-medium">Nom</label>
        <input
          type="text"
          v-model="local.nom"
          @input="emitChange"
          class="w-full border rounded px-3 py-2"
          placeholder="Votre nom"
        />
      </div>
      <!-- Prénom -->
      <div>
        <label class="block mb-1 font-medium">Prénom</label>
        <input
          type="text"
          v-model="local.prenom"
          @input="emitChange"
          class="w-full border rounded px-3 py-2"
          placeholder="Votre prénom"
        />
      </div>
      <!-- Téléphone -->
      <div>
        <label class="block mb-1 font-medium">Téléphone</label>
        <input
          type="text"
          v-model="local.telephone"
          @input="emitChange"
          class="w-full border rounded px-3 py-2"
          placeholder="06 12 34 56 78"
        />
      </div>
      <!-- Email -->
      <div>
        <label class="block mb-1 font-medium">Email</label>
        <input
          type="email"
          v-model="local.email"
          @input="emitChange"
          class="w-full border rounded px-3 py-2"
          placeholder="votre@email.com"
        />
      </div>
      <!-- Consentement -->
      <div class="flex items-center">
        <input
          type="checkbox"
          id="consent"
          v-model="local.consent"
          @change="emitChange"
          class="mr-2"
        />
        <label for="consent">J'accepte la collecte de mes données personnelles</label>
      </div>
    </form>
  </div>
</template>

<script setup lang="ts">
import { defineProps, defineEmits, watch, ref } from 'vue'

type APropos = {
  nom: string
  prenom: string
  telephone: string
  email: string
  consent: boolean
}

const props = defineProps<{ modelValue: Partial<APropos> }>()
const emit = defineEmits(['update:modelValue'])

const local = ref<Partial<APropos>>({
  nom: props.modelValue.nom || '',
  prenom: props.modelValue.prenom || '',
  telephone: props.modelValue.telephone || '',
  email: props.modelValue.email || '',
  consent: props.modelValue.consent || false,
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
