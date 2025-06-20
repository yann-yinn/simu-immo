<template>
  <div class="max-w-xl mx-auto p-6 bg-white rounded shadow">
    <div v-if="step === 1">
      <StepAdresse v-model="form.adresse" />
    </div>
    <div v-else-if="step === 2">
      <StepTypeBien v-model="form.typeBien" />
    </div>
    <div v-else-if="step === 3">
      <StepPrecisions v-model="form.precisions" />
    </div>
    <div v-else-if="step === 4">
      <StepCaracteristiques v-model="form.caracteristiques" />
    </div>
    <div v-else-if="step === 5">
      <StepAPropos v-model="form.aPropos" />
    </div>
    <div class="flex justify-between mt-6">
      <button v-if="step > 1" @click="prevStep" class="px-4 py-2 bg-gray-200 rounded">
        Précédent
      </button>
      <button v-if="step < 5" @click="nextStep" class="px-4 py-2 bg-blue-600 text-white rounded">
        Suivant
      </button>
      <button
        v-if="step === 5"
        @click="submitForm"
        class="px-4 py-2 bg-green-600 text-white rounded"
      >
        Envoyer
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import StepAdresse from '../components/StepAdresse.vue'
import StepTypeBien from '../components/StepTypeBien.vue'
import StepPrecisions from '../components/StepPrecisions.vue'
import StepCaracteristiques from '../components/StepCaracteristiques.vue'
import StepAPropos from '../components/StepAPropos.vue'

const step = ref(1)
const form = ref({
  adresse: {},
  typeBien: {},
  precisions: {},
  caracteristiques: {},
  aPropos: {},
})

function nextStep() {
  if (step.value < 5) step.value++
}
function prevStep() {
  if (step.value > 1) step.value--
}
function submitForm() {
  // TODO: envoyer les données
  alert('Formulaire envoyé !')
}
</script>
