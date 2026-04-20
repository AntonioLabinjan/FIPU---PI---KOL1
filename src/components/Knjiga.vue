<script setup>
defineProps({
  naslov: String,
  autor: String,
  godina: Number,
  naslovnica: String
})

const emit = defineEmits(['promjeni'])
</script>

<template>
  <div>
    <img :src="naslovnica" :alt="naslov" style="height: 300px;" />

    <p>
      <slot />
      <strong>Naslov:</strong> {{ naslov }}
    </p>
    <p><strong>Autor:</strong> {{ autor }}</p>
    <p><strong>Godina:</strong> {{ godina }}</p>

    <button @click="emit('promjeni', -1)">Prethodna</button>
    <button @click="emit('promjeni', 1)">Sljedeća</button>
  </div>
</template>