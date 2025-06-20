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
        <div>
          {{
            typeof props.adresse === 'object' && props.adresse !== null
              ? props.adresse.label
              : props.adresse
          }}
        </div>
      </div>
      <!-- Type de bien -->
      <div>
        <h3 class="font-semibold">Informations principales</h3>
        <ul class="ml-4 list-disc">
          <li>Type : {{ props.typeBien.type }}</li>
          <li>Surface : {{ props.typeBien.surface }} m²</li>
          <li>Année de construction : {{ props.typeBien.annee }}</li>
          <li>Diagnostic : {{ diagnosticLabel(props.typeBien.diagnostic) }}</li>
          <li>État : {{ etatLabel(props.typeBien.etat) }}</li>
        </ul>
      </div>
      <!-- Précisions -->
      <div>
        <h3 class="font-semibold">Précisions</h3>
        <ul class="ml-4 list-disc">
          <li>Nombre de pièces : {{ props.precisions.pieces }}</li>
          <li>Nombre de chambres : {{ props.precisions.chambres }}</li>
          <li>Nombre de salles de bain : {{ props.precisions.sdb }}</li>
          <li>Nombre d'étages dans l'immeuble : {{ props.precisions.etagesImmeuble }}</li>
          <li>Étage de ce bien : {{ props.precisions.etageBien }}</li>
        </ul>
      </div>
      <!-- Caractéristiques -->
      <div>
        <h3 class="font-semibold">Caractéristiques</h3>
        <ul class="ml-4 list-disc">
          <li v-for="k in caracteristiquesCochees as string[]" :key="k">
            {{ caracteristiqueLabel(k) }}
          </li>
          <li v-if="caracteristiquesCochees.length === 0">Aucune</li>
        </ul>
      </div>
      <!-- À propos -->
      <div>
        <h3 class="font-semibold">À propos de vous</h3>
        <ul class="ml-4 list-disc">
          <li>Nom : {{ props.aPropos.nom }}</li>
          <li>Prénom : {{ props.aPropos.prenom }}</li>
          <li>Téléphone : {{ props.aPropos.telephone }}</li>
          <li>Email : {{ props.aPropos.email }}</li>
          <li>
            Consentement : <span v-if="props.aPropos.consent">Oui</span><span v-else>Non</span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'

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
interface APropos {
  nom?: string
  prenom?: string
  telephone?: string
  email?: string
  consent?: boolean
}

const props = defineProps<{
  adresse: AdresseSuggestion | string
  typeBien: TypeBien
  precisions: PrecisionsBien
  caracteristiques: Record<string, boolean>
  aPropos: APropos
}>()

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
const caracteristiquesCochees = computed<string[]>(() =>
  Object.entries(props.caracteristiques)
    .filter(([k, v]) => !!k && v === true)
    .map(([k]) => k),
)
</script>
