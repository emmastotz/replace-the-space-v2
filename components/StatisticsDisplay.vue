<template>
  <div class="grid grid-cols-2 text-white font-semibold uppercase">
    <span>
      Wins: {{ stats.wins }}
    </span>
    <span>
      Guesses Left: {{ guessesLeft }}
    </span>
    <span>
      Losses: {{ stats.losses }}
    </span>
    <div v-if="strikes.length >= 0">
      Strikes:
      <span v-for="letter of strikes" :key="letter">
        {{ letter }}
      </span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    stats: {
      type: Object,
      required: true
    },
    guessesLeft: {
      type: Number,
      required: true
    },
    strikes: {
      type: Array,
      required: true
    }
  }
}
</script>
