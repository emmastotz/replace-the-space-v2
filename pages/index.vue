<template>
  <main class="h-screen">
    <div class="bg-gradient-to-tr from-red-600 to-red-900 flex items-center justify-center py-16 w-full">
      <div class="grid gap-12 w-full justify-center">
        <div class="flex space-x-2">
          <div v-for="letter of splitWord" :key="letter" class="flex border-b-2 border-white font-semibold text-lg text-white justify-center uppercase sm:px-6 sm:py-4">
            {{ isRevealed(letter) }}
          </div>
        </div>

        <div class="flex space-x-5 font-semibold uppercase text-white">
          <h2 class="tracking-widest">
            Strikes:
          </h2>
          <ul v-for="strike of strikes" :key="strike.guess">
            <li>
              {{ strike.icon }}
            </li>
          </ul>
        </div>

        <div class="grid grid-cols-9 grid-flow-row gap-1 sm:gap-2 lg:gap-4">
          <div v-for="letter of letters" :key="letter">
            <button
              class="w-full text-white uppercase font-semibold text-lg rounded-md px-4 py-2 hover:shadow-inner hover:bg-red-500"
              :class="{'bg-red-900 shadow-inner' : guesses.includes(letter)}"
              :disabled="guesses.includes(letter) || gameOver"
              @click="guess(letter)"
            >
              <span>{{ letter }}</span>
            </button>
          </div>
        </div>

        <div>
          <p class="text-white font-semibold uppercase tracking-widest">
            {{ message }}
          </p>
        </div>
      </div>
    </div>
    <div class="flex flex-col space-y-6 justify-center bg-gradient-to-tl from-blue-900 via-blue-700 to-blue-400">
      <div class="flex space-x-8 text-white font-semibold uppercase bg-blue-200 text-blue-600 p-2 justify-center">
        <span>
          Wins: {{ stats.wins }}
        </span>
        <span>
          Losses: {{ stats.losses }}
        </span>
      </div>
      <div class="flex justify-center">
        <button class="uppercase bg-blue-200 text-blue-600 font-semibold tracking-widest text-lg rounded-md px-4 py-1 shadow hover:shadow-lg hover:bg-white" :class="{'bg-orange-500 text-white': gameOver}" @click="newGame">
          New Game
        </button>
      </div>
      <div class="px-12 pb-40">
        <img
          class="object-cover lg:max-w-8xl"
          src="@/assets/images/replacethespace.png"
          alt="Replace the Space Logo"
        >
      </div>
    </div>
  </main>
</template>

<script>
const allowedStrikes = 3
const defaultStrikes = new Array(allowedStrikes).fill({ icon: '⚪', guess: '' })

export default {
  data () {
    return {
      letters: ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z'],
      words: ['galaxy', 'neptune', 'blackhole', 'moon', 'eclipse', 'asteroid', 'jupiter', 'astronaut', 'telescope', 'mercury', 'density', 'saturn', 'mars', 'sun', 'star', 'comet', 'venus', 'uranus', 'earth', 'space', 'pluto', 'astronomy', 'equinox', 'cosmos', 'gravity'],
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
  created () {
    this.generateRandomWord()
  },
  // mounted () {
  //   window.addEventListener('keypress', e => this.handleKeyPress(e))
  // },
  // destroyed () {
  //   window.removeEventListener('keypress', e => this.handleKeyPress(e))
  // },
  methods: {
    // handleKeyPress (e) {
    //   const key = e.key.toUpperCase()
    //   if (key.length === 1 && key.match(/[a-zA-Z]/) && !this.guesses.includes(key)) {
    //     this.guess(key)
    //   }
    // },
    generateRandomWord () {
      this.currentWord = this.words[Math.floor(Math.random() * this.words.length)]
    },
    isRevealed (letter) {
      if (!letter.match(/[a-zA-Z\s]/)) {
        return letter
      }
      return this.guesses.includes(letter) || this.gameOver ? letter : '?'
    },
    guess (letter) {
      this.guesses.push(letter)
      if (!this.currentWord.includes(letter)) {
        this.strikes = [{ icon: '🚫', guess: letter }, ...this.strikes.slice(0, 2)]
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
