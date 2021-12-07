<template>
  <div class="grid text-white font-semibold uppercase">
    <span>
      Wins: {{ stats.wins }}
    </span>
    <span>
      Losses: {{ stats.losses }}
    </span>
    <span>
      Guesses Left: {{ stats.guessesLeft }}
    </span>
    <div v-if="lettersGuessed.length > 0">
      Letters Guessed:
      <span v-for="letter of lettersGuessed" :key="letter">
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
    lettersGuessed: {
      type: Array,
      required: true
    }
  }
}
</script>
