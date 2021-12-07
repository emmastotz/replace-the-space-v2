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
    <div v-if="incorrectGuesses.length >= 0">
      Incorrect Guesses:
      <span v-for="letter of incorrectGuesses" :key="letter">
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
    incorrectGuesses: {
      type: Array,
      required: true
    }
  }
}
</script>
