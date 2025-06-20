<template>
  <div>
    <div class="bg-green-100 text-green-800 px-4 py-2 rounded mb-6 text-center font-semibold">
      Votre formulaire a bien été transmis, merci !
    </div>
    <h2 class="text-2xl font-bold mb-4">Récapitulatif de votre simulation</h2>
    <div class="space-y-4">
      <!-- Adresse -->
      <div>
        <h3 class="font-semibold">Adresse</h3>
        <div>{{ adresse.label || adresse }}</div>
      </div>
      <!-- Type de bien -->
      <div>
        <h3 class="font-semibold">Informations principales</h3>
        <ul class="ml-4 list-disc">
          <li>Type : {{ typeBien.type }}</li>
          <li>Surface : {{ typeBien.surface }} m²</li>
          <li>Année de construction : {{ typeBien.annee }}</li>
          <li>Diagnostic : {{ diagnosticLabel(typeBien.diagnostic) }}</li>
          <li>État : {{ etatLabel(typeBien.etat) }}</li>
        </ul>
      </div>
      <!-- Précisions -->
      <div>
        <h3 class="font-semibold">Précisions</h3>
        <ul class="ml-4 list-disc">
          <li>Nombre de pièces : {{ precisions.pieces }}</li>
          <li>Nombre de chambres : {{ precisions.chambres }}</li>
          <li>Nombre de salles de bain : {{ precisions.sdb }}</li>
          <li>Nombre d'étages dans l'immeuble : {{ precisions.etagesImmeuble }}</li>
          <li>Étage de ce bien : {{ precisions.etageBien }}</li>
        </ul>
      </div>
      <!-- Caractéristiques -->
      <div>
        <h3 class="font-semibold">Caractéristiques</h3>
        <ul class="ml-4 list-disc">
          <li v-for="(v, k) in caracteristiques" :key="k" v-if="v">
            {{ caracteristiqueLabel(k) }}
          </li>
          <li v-if="!hasCaracteristique">Aucune</li>
        </ul>
      </div>
      <!-- À propos -->
      <div>
        <h3 class="font-semibold">À propos de vous</h3>
        <ul class="ml-4 list-disc">
          <li>Nom : {{ aPropos.nom }}</li>
          <li>Prénom : {{ aPropos.prenom }}</li>
          <li>Téléphone : {{ aPropos.telephone }}</li>
          <li>Email : {{ aPropos.email }}</li>
          <li>Consentement : <span v-if="aPropos.consent">Oui</span><span v-else>Non</span></li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, defineProps } from 'vue'

interface AdresseSuggestion {
  label?: string
  [key: string]: any
}
interface TypeBien {
  type?: string
  surface?: number | null
  annee?: number | ''
  diagnostic?: string
  etat?: string
}
interface PrecisionsBien {
  pieces?: number | null
  chambres?: number | null
  sdb?: number | null
  etagesImmeuble?: number | null
  etageBien?: number | null
}
interface Caracteristiques {
  [key: string]: boolean
}
interface APropos {
  nom?: string
  prenom?: string
  telephone?: string
  email?: string
  consent?: boolean
}

defineProps<{
  adresse: AdresseSuggestion | string
  typeBien: TypeBien
  precisions: PrecisionsBien
  caracteristiques: Caracteristiques
  aPropos: APropos
}>()

const props = defineProps()

function diagnosticLabel(val: string) {
  if (val === 'unknown') return 'Je ne sais pas'
  return val
}
function etatLabel(val: string) {
  switch (val) {
    case 'standard':
      return 'Standard'
    case 'refait':
      return 'Refait à neuf'
    case 'rafraichissement':
      return 'Rafraichissement nécessaire'
    case 'travaux':
      return 'Travaux importants'
    default:
      return val
  }
}
const caracteristiqueLabels: Record<string, string> = {
  ascenseur: 'Ascenseur',
  cave: 'Cave',
  balcon: 'Balcon',
  terrasse: 'Terrasse',
  parking: 'Parking',
  vue: 'Vue exceptionnelle',
  calme: 'Logement calme',
  ravalement: 'Ravalement de façade récent',
}
function caracteristiqueLabel(key: string) {
  return caracteristiqueLabels[key] || key
}
const hasCaracteristique = computed(() => {
  return Object.values(props.caracteristiques).some((v: unknown) => Boolean(v))
})
</script>
