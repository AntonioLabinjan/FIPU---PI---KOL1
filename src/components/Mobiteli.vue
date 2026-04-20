<script setup>
import { ref, computed } from 'vue'

const iphone_funkcionalnosti = ['Face ID', 'Apple Pay', 'Siri', 'AirDrop']
const samsung_funkcionalnosti = ['Samsung Pay', 'Bixby', 'DeX', 'S Pen']

const mobiteli = ref([
  { marka: 'Apple', model: 'iPhone 15', cijena: 1000, funkcionalnosti: ['Face ID', 'Apple Pay'] },
  { marka: 'Samsung', model: 'Galaxy S22', cijena: 900, funkcionalnosti: ['Bixby', 'Samsung Pay'] },
  { marka: 'Google', model: 'Pixel 8', cijena: 850, funkcionalnosti: ['Google Assistant'] },
  { marka: 'Samsung', model: 'Galaxy Z Fold 5', cijena: 1800, funkcionalnosti: ['Apple Pay', 'S Pen', 'DeX'] }
])

const novaCijena = ref(333)

function azurirajCijenu(mobitelIndex) {
  if (mobitelIndex !== -1) {
    mobiteli.value[mobitelIndex].cijena = novaCijena.value
  }
}

function provjeriFunkcionalnost(mobitelIndex, funkcionalnostIndex) {
  const mobitel = mobiteli.value[mobitelIndex]
  // Uklanjamo stare simbole ako postoje da dobijemo čisti naziv
  const originalnaFunkcionalnost = mobitel.funkcionalnosti[funkcionalnostIndex]
    .replace(' ✅', '')
    .replace(' ❌', '')

  let validneFunkcionalnosti = []

  if (mobitel.marka === 'Apple') {
    validneFunkcionalnosti = iphone_funkcionalnosti
  } else if (mobitel.marka === 'Samsung') {
    validneFunkcionalnosti = samsung_funkcionalnosti
  } else {
    return
  }

  const jeValidna = validneFunkcionalnosti.includes(originalnaFunkcionalnost)

  // ISPRAVLJENO: Korištenje backtickova za template literals
  mobitel.funkcionalnosti[funkcionalnostIndex] = jeValidna
    ? `${originalnaFunkcionalnost} ✅`
    : `${originalnaFunkcionalnost} ❌`
}

function imaProvjeru(funkcionalnost) {
  return funkcionalnost.includes('✅') || funkcionalnost.includes('❌')
}

function simbolMarke(marka) {
  if (marka === 'Apple') return '🍎'
  if (marka === 'Samsung') return '🔵'
  return '❓'
}

const sortiraniMobiteli = computed(() => {
  // Kreiramo kopiju polja prije sortiranja kako ne bismo mutirali originalni ref direktno u computedu
  return [...mobiteli.value].sort((a, b) => b.cijena - a.cijena)
})
</script>

<template>
  <div>
    <h2>Zadatak 2 - Mobiteli</h2>

    <div>
      <label>Nova cijena:</label>
      <input v-model="novaCijena" type="number" />
    </div>

    <div
      v-for="mobitel in sortiraniMobiteli"
      :key="mobitel.model"
      style="border: 1px solid #ccc; padding: 12px; margin: 12px 0;"
    >
      <p>{{ simbolMarke(mobitel.marka) }} <strong>Marka:</strong> {{ mobitel.marka }}</p>
      <p><strong>Model:</strong> {{ mobitel.model }}</p>
      <p>
        <strong>Cijena:</strong> {{ mobitel.cijena }} €
        <button @click="azurirajCijenu(mobiteli.findIndex(m => m.model === mobitel.model))">
          Ažuriraj
        </button>
      </p>

      <p><strong>Funkcionalnosti:</strong></p>
      <ul>
        <li v-for="(funkcionalnost, fIndex) in mobitel.funkcionalnosti" :key="fIndex">
          {{ funkcionalnost }}

          <template v-if="mobitel.marka === 'Apple' || mobitel.marka === 'Samsung'">
            <button
              v-if="!imaProvjeru(funkcionalnost)"
              @click="provjeriFunkcionalnost(mobiteli.findIndex(m => m.model === mobitel.model), fIndex)"
            >
              Provjeri
            </button>
          </template>

          <template v-else>
            <span>*Provjera nije moguća!</span>
          </template>
        </li>
      </ul>
    </div>
  </div>
</template>