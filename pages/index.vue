<template>
  <main class="bg-gradient-to-tl from-blue-900 via-blue-700 to-blue-400 h-screen overflow-hidden">
    <div class="bg-gradient-to-tr from-red-600 to-red-900 flex items-center justify-center py-16 w-full">
      <div class="grid gap-8 w-full justify-center">
        <div class="grid gap-1 sm:gap-2 lg:gap-4 max-w-7xl" :class="`grid-cols-${currentWord.length}`">
          <div v-for="letter of splitWord" :key="letter" class="border-b-2 border-white font-semibold text-lg text-white sm:px-6 sm:py-4 justify-center">
            {{ isRevealed(letter) }}
          </div>
        </div>

        <div class="flex space-x-5 font-semibold uppercase text-white">
          <h2>
            Strikes:
          </h2>
          <ul v-for="strike of strikes" :key="strike.guess">
            <li>
              {{ strike.icon }}
            </li>
          </ul>
        </div>

        <div class="grid grid-cols-9 grid-flow-row gap-1 sm:gap-2 lg:gap-4">
          <button
            v-for="letter of letters"
            :key="letter"
            class="w-full bg-red-100 rounded-md uppercase text-red-600 font-semibold text-lg rounded-md px-4 py-2 shadow hover:shadow-lg hover:bg-white disabled:opacity-50"
            :class="{'bg-orange-500 text-white': badGuesses.includes(letter), 'bg-green-500 text-white': guesses.includes(letter)}"
            :disabled="guesses.includes(letter) || gameOver"
            @click="guess(letter)"
          >
            <span>{{ letter }}</span>
          </button>
        </div>

        <div>
          <p class="text-white font-semibold">
            {{ message }}
          </p>
        </div>
      </div>
    </div>
    <div class="flex flex-col space-y-6 justify-center">
      <StatisticsDisplay
        :stats="stats"
      />
      <div class="flex justify-center">
        <button class="uppercase bg-blue-200 text-blue-600 font-semibold tracking-widest text-lg rounded-md px-4 py-1 shadow hover:shadow-lg hover:bg-white" :class="{'bg-orange-500 text-white': gameOver}" @click="newGame">
          New Game
        </button>
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
import StatisticsDisplay from '@/components/StatisticsDisplay.vue'

const allowedStrikes = 3

const defaultStrikes = new Array(allowedStrikes).fill({ icon: '⚪', guess: '' })

export default {
  components: { StatisticsDisplay },
  data () {
    return {
      letters: ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z'],
      words: ['galaxy', 'neptune', 'blackhole', 'moon', 'eclipse', 'asteroid', 'jupiter', 'astronaut', 'telescope', 'mercury', 'density', 'saturn', 'mars', 'sun', 'star', 'comet', 'venus', 'uranus', 'earth', 'space', 'pluto', 'astronomy', 'equinox', 'cosmos', 'constellation', 'gravity'],
      currentWord: '',
      guesses: [],
      strikes: [...defaultStrikes],
      gameOver: false,
      stats: {
        wins: 0,
        losses: 0
      }
    }
  },
  computed: {
    splitWord () {
      return this.currentWord.split('')
    },
    badGuesses () {
      return this.strikes.filter(strike => strike.guess).map(strike => strike.guess)
    },
    strikeout () {
      return this.badGuesses.length >= allowedStrikes
    },
    puzzleComplete () {
      return this.unrevealed === 0
    },
    unrevealed () {
      return [...this.currentWord].filter((letter) => {
        return letter.match(/[a-zA-Z]/) && !this.guesses.includes(letter)
      }).length
    },
    message () {
      if (!this.gameOver) {
        return '☝️ Pick a letter'
      } else if (this.strikeout) {
        return '❌ You lost this round. Try again?'
      } else if (this.puzzleComplete) {
        return '🎉 You win!'
      }
      return '😬 Unforeseen error state, maybe try a new game?'
    }
  },
  mounted () {
    this.generateRandomWord()
    window.addEventListener('keypress', e => this.handleKeyPress(e))
  },
  destroyed () {
    window.removeEventListener('keypress', e => this.handleKeyPress(e))
  },
  methods: {
    // Allows user to enter guesses with the keyboard
    handleKeyPress (e) {
      const key = e.key.toLowerCase()
      if (key.length === 1 && this.alphabet.includes(key) && !this.lettersGuessed.includes(key)) {
        this.userGuess = key
      }

      this.guess(this.userGuess)
    },
    // Generates random word from the list
    generateRandomWord () {
      this.currentWord = this.words[Math.floor(Math.random() * this.words.length)]
    },
    // Turns unguessed letters into blank spaces
    isRevealed (letter) {
      if (!letter.match(/[a-zA-Z\s]/)) {
        return letter
      }
      return this.guesses.includes(letter) || this.gameOver ? letter : ''
    },
    // Handles the guess and all possible results
    guess (letter) {
      this.guesses.push(letter)
      if (!this.currentWord.includes(letter)) {
        this.strikes = [{ icon: '🚫', guess: letter }, ...this.strikes]
      }
      if (this.strikeout) {
        this.gameOver = true
        this.stats.losses++
      }
      if (this.puzzleComplete) {
        this.gameOver = true
        this.stats.wins++
      }
    },
    // Allows player to start a new game
    newGame () {
      const confirmation = confirm('End this game and start a new one?')
      if (!confirmation) { return }
      this.generateRandomWord()
      this.guesses = []
      this.strikes = [...defaultStrikes]
      this.gameOver = false
    }
  }
}
</script>
