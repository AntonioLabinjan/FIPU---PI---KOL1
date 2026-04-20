<script setup>
import { ref, computed } from 'vue'

const email = ref('')
const lozinka = ref('')
const ponovi_lozinku = ref('')

//function provjeraEmaila(){}
//function provjeraLozinke(){}


function provjeraEmaila() {
  if (!email.value.includes('@')) { // ok
    return 1
  }

  const dijelovi = email.value.split('@')

  if (dijelovi.length !== 2 || !dijelovi[0] || !dijelovi[1]) {
    return 2 // ok
  }

  const domena = dijelovi[1]

  if (domena !== 'unipu.hr' && domena !== 'student.unipu.hr') {
    return 3 // ok
  }

  return 4
}

const provjeraLozinke = computed(() => {
  if (lozinka.value.length < 8) {
    return 'Lozinka mora imati najmanje 8 znakova!'
  }

  if (!/[A-Z]/.test(lozinka.value)) {
    return 'Lozinka mora imati barem jedno veliko slovo!'
  }

  return 'Lozinka je validna!'
})

/*
Razlika:
computed funkcija se cachea i ponovno izračunava samo kada se promijeni
ovisnost (ovdje lozinka), dok bi se obična funkcija pozivala iznova pri
svakom renderiranju templatea. U ovom zadatku computed je bolji izbor jer
je provjera lozinke vezana direktno uz reaktivnu vrijednost lozinka.
*/
</script>

<template>
  <div>
    <h2>Zadatak 1 - Validacija forme</h2>

    <div>
      <label>Email:</label>
      <input v-model="email" type="text" />
    </div>

    <div>
      <label>Lozinka:</label>
      <input v-model="lozinka" type="password" />
    </div>

    <div>
      <label>Ponovi lozinku:</label>
      <input v-model="ponovi_lozinku" type="password" />
    </div>

    <div>
      <p v-if="provjeraEmaila() === 1">Email treba sadržavati "@" simbol!</p>
      <p v-else-if="provjeraEmaila() === 2">Email se treba sastojati od lokalnog djela i domene!</p>
      <p v-else-if="provjeraEmaila() === 3">Email treba završiti s "unipu.hr" ili "student.unipu.hr"!</p>
      <p v-else>Email je validan!</p>
    </div>

    <div>
      <p>{{ provjeraLozinke }}</p>
    </div>

    <div v-if="ponovi_lozinku !== '' && lozinka !== ponovi_lozinku">
      <p>Lozinke se ne podudaraju!</p>
    </div>
  </div>
</template>