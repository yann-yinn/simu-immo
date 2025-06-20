<template>
  <div class="max-w-xl mx-auto p-6 bg-white rounded shadow">
    <template v-if="!submitted">
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
    </template>
    <template v-else>
      <RecapitulatifFormulaire
        :adresse="form.adresse as any"
        :typeBien="form.typeBien as any"
        :precisions="form.precisions as any"
        :caracteristiques="form.caracteristiques as any"
        :aPropos="form.aPropos as any"
      />
    </template>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import StepAdresse from '../components/StepAdresse.vue'
import StepTypeBien from '../components/StepTypeBien.vue'
import StepPrecisions from '../components/StepPrecisions.vue'
import StepCaracteristiques from '../components/StepCaracteristiques.vue'
import StepAPropos from '../components/StepAPropos.vue'
import RecapitulatifFormulaire from '../components/RecapitulatifFormulaire.vue'

const step = ref(1)
const submitted = ref(false)
const form = ref({
  adresse: '',
  typeBien: {
    type: '',
    surface: null,
    annee: '' as number | '',
    diagnostic: '',
    etat: '',
  },
  precisions: {
    pieces: null,
    chambres: null,
    sdb: null,
    etagesImmeuble: null,
    etageBien: null,
  },
  caracteristiques: {
    ascenseur: false,
    cave: false,
    balcon: false,
    terrasse: false,
    parking: false,
    vue: false,
    calme: false,
    ravalement: false,
  },
  aPropos: {
    nom: '',
    prenom: '',
    telephone: '',
    email: '',
    consent: false,
  },
})

function nextStep() {
  if (step.value < 5) step.value++
}
function prevStep() {
  if (step.value > 1) step.value--
}
function submitForm() {
  submitted.value = true
}
</script>
