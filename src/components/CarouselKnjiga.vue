<script setup>
import { ref } from 'vue'
import Knjiga from './Knjiga.vue'

const knjige = [
  {
    autor: 'George Orwell',
    godina: 1949,
    naslov: '1984',
    naslovnica: '/src/assets/naslovnice/1984.png'
  },
  {
    autor: 'Harper Lee',
    godina: 1960,
    naslov: 'To Kill a Mockingbird',
    naslovnica: '/src/assets/naslovnice/To Kill a Mockingbird.png'
  },
  {
    autor: 'Mary Shelley',
    godina: 1818,
    naslov: 'Frankenstein',
    naslovnica: '/src/assets/naslovnice/Frankenstein.png'
  },
  {
    autor: 'J.R.R. Tolkien',
    godina: 1954,
    naslov: 'The Fellowship of the Ring',
    naslovnica: '/src/assets/naslovnice/The Fellowship of the Ring.png'
  }
]

const aktivnaKnjiga = ref(0)

function promjeniKnjigu(n) {
  aktivnaKnjiga.value += n

  if (aktivnaKnjiga.value < 0) {
    aktivnaKnjiga.value = knjige.length - 1
  }

  if (aktivnaKnjiga.value >= knjige.length) {
    aktivnaKnjiga.value = 0
  }
}
</script>

<template>
  <div>
    <h2>Zadatak 3 - Carousel knjiga</h2>

    <Knjiga
      :naslov="knjige[aktivnaKnjiga].naslov"
      :autor="knjige[aktivnaKnjiga].autor"
      :godina="knjige[aktivnaKnjiga].godina"
      :naslovnica="knjige[aktivnaKnjiga].naslovnica"
      @promjeni="promjeniKnjigu"
    >
      {{ aktivnaKnjiga + 1 }}.
    </Knjiga>
  </div>
</template>