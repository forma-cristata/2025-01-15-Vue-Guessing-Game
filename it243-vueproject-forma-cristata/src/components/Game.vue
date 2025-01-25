

<template>
  <div id="guessing-div" class="block">
    <div id="upper-bound-div" class="d-inline-block w-50">
      <label>
        Upper Bound
        <input id="upper-bound-input" class="w-25 inputs" type="number" @blur="generateNumber" v-model="upperBound" min="1" required />
      </label>
    </div>
    <div id="guesses-count-div" class="d-inline-block w-50">
      <label>
        Number of Guesses
        <input id="guesses-count-input" class="w-25 inputs" type="number" v-model="guessCount" min="1" required />
      </label>
    </div>
    <hr />
    <form @keydown.enter.prevent="submit()">

      <div id="guessed-number">
        <label>
          Guess a number
          <input id="guess-input" type="number" class="w-25 inputs" v-model="guess" required disabled />
        </label>
      </div>

    </form>
    <hr />
    <div class="block">{{ previousGuesses }}</div>
  </div>
</template>



<script>
export default {
  name: "Game",
  data() {
    return {
      guess: '',
      upperBound: 0,
      guessCount: 0,
      previousGuesses: "Previous Guesses: ",
      answer: -1,
    }
    // This is strange, but the way I wrote this does not require a default value because
    // the user physically cannot start guessing without inputting an upper bound.
  },
  methods: {
    generateNumber(){
      if(this.upperBound > 1) {
        this.answer = Math.floor(Math.random() * (this.upperBound + 1));
        console.log(this.answer);
        document.querySelector("#upper-bound-input").disabled = true;
        document.querySelector('#guess-input').disabled = false;
        let autoCalcGuessCount = Math.round(this.upperBound / 20);
        document.querySelector('#guesses-count-input').setAttribute('placeholder', `${autoCalcGuessCount}`);
      }
    },
    displayResults(){
      return null; //TODO
      // Erase the page
      // Display Results
      // Reset the game if the user wishes.
    },
    checkGuess(x) {
      if(this.previousGuesses === "Previous Guesses: ")
      {
        this.generateNumber();
      }
      if(this.previousGuesses.split('|').length >= this.guessCount)
      {
        let allInputs = document.getElementsByClassName('inputs');
        for(let i = 0; i < allInputs.length; i++)
        {
          allInputs[i].disabled = true;
        }
        displayResults();
      }
      else {
        if (x > this.answer) {
          return "Guess is too high.";
        } else if (x < this.answer) {
          return "Guess is too low";
        } else {
          return "Guess is correct"
        }
      }
    },
    postResult(result, currentGuess) {
      // Add to guesses array
      this.previousGuesses += `${currentGuess} : ${result} | `;
      // Add to guesses HTML
      // ??
      //Test it is working
      console.log(currentGuess);
      console.log(result);
    },
    submit() {
      let result = this.checkGuess(parseInt(this.guess));
      this.postResult(result, this.guess);
      this.$emit('submit', this.guess);
    }

  }
}
</script>




<style scoped>

</style>