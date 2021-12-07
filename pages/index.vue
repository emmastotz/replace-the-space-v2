<template>
  <main class="bg-gradient-to-tl from-blue-900 via-blue-700 to-blue-400 h-screen overflow-hidden">
    <div class="bg-gradient-to-tr from-red-600 to-red-900 flex items-center justify-center py-16">
      <button v-if="!isPlaying" class="uppercase bg-red-100 text-red-600 font-semibold tracking-widest text-lg rounded-md px-5 py-1 shadow hover:shadow-lg hover:bg-white" @click="show">
        Play
      </button>
      <div v-else class="grid gap-6">
        <div class="grid gap-1 sm:gap-2 lg:gap-4" :class="`grid-cols-${randomWord.wordLength}`">
          <div v-for="letter of randomWord.letters" :key="letter.id">
            <BlankSpaces :value="letter" />
          </div>
        </div>
        <StatisticsDisplay
          :stats="stats"
          :incorrect-guesses="incorrectGuesses"
          :guesses-left="guessesLeft"
        />
      </div>
    </div>
    <div class="flex px-12 justify-center">
      <img
        class="object-cover lg:max-w-8xl"
        src="@/assets/images/replacethespace.png"
        alt="Replace the Space Logo"
      >
    </div>
  </main>
</template>

<script>
import BlankSpaces from '@/components/BlankSpaces.vue'
import StatisticsDisplay from '@/components/StatisticsDisplay.vue'

export default {
  components: { BlankSpaces, StatisticsDisplay },
  data () {
    return {
      isPlaying: false,
      gameOver: false,
      isCorrect: false,
      randomWord: {
        word: '',
        wordLength: 0,
        letters: []
      },
      userGuess: '',
      isValid: false,
      stats: {
        wins: 0,
        losses: 0
      },
      guessesLeft: 5,
      lettersGuessed: [],
      incorrectGuesses: [],
      wordLibrary: ['galaxy', 'neptune', 'blackhole', 'moon', 'eclipse', 'asteroid', 'jupiter', 'astronaut', 'telescope', 'mercury', 'density', 'saturn', 'mars', 'sun', 'star', 'comet', 'venus', 'uranus', 'earth', 'space', 'pluto', 'astronomy', 'equinox', 'cosmos', 'constellation', 'gravity'],
      alphabet: ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z']
    }
  },
  created () {
    this.initialize()
  },
  mounted () {
    window.addEventListener('keypress', (e) => {
      const key = e.key.toLowerCase()
      if (key.length === 1 && this.alphabet.includes(key) && !this.lettersGuessed.includes(key)) {
        this.userGuess = key
      }

      this.guess(this.userGuess)
    })
  },
  methods: {
    show () {
      this.isPlaying = !this.isPlaying
    },
    initialize () {
      // reset random word
      this.generateRandomWord()

      // reset game over flag
      this.gameOver = false

      // reset letters guessed array
      this.lettersGuessed = []

      // reset incorrect guesses array
      this.incorrectGuesses = []

      // reset guesses left
      this.guessesLeft = 5
    },
    generateRandomWord () {
      this.randomWord.word = this.wordLibrary[Math.floor(Math.random() * this.wordLibrary.length)]
      this.randomWord.wordLength = this.randomWord.word.length
      for (let i = 0; i < this.randomWord.wordLength; i++) {
        this.randomWord.letters.push({
          id: i,
          letter: this.randomWord.word[i],
          isShowing: false
        })
      }
    },
    guess (guess) {
      this.lettersGuessed.push(guess)

      for (let i = 0; i <= this.randomWord.wordLength; i++) {
        if (guess === this.randomWord.word[i]) {
          this.showLetter(i)
          this.isCorrect = true
        }
      }

      this.randomWord.wordLength--

      if (!this.isCorrect && this.guessesLeft > 0 && !this.lettersGuessed.includes(guess)) {
        this.guessesLeft--
        this.incorrectGuesses.push(guess)
      }

      this.isCorrect = false

      if (this.guessesLeft === 0 && this.randomWord.wordLength > 0) {
        this.lose()
      }

      if (this.guessesLeft > 0 && this.randomWord.wordLength === 0) {
        this.win()
      }
    },
    showLetter (i) {
      this.randomWord.letters[i].isShowing = !this.randomWord.letters[i].isShowing
    },
    lose () {
      this.stats.losses++
      const playAgain = confirm('You lose. The correct answer was ' + this.randomWord.word + '. Would you like to play again?')
      if (playAgain) {
        this.initialize()
      }
    },
    win () {
      this.stats.wins++
      const playAgain = confirm('You won. Keep the streak alive?')
      if (playAgain) {
        this.initialize()
      }
    }
  }
}
</script>
