<template>
  <div>
    <div
      class="number"
      :id="'number-' + number"
      v-for="number in n()"
      :key="number"
      @mouseover="hov(number)"
      @mouseout="reset"
    >
      {{ number }}
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      limit: this.$parent.limit,
      numbers: []
    };
  },
  watch: {
    ["$parent.limit"](newLimit) {
      this.limit = newLimit;
    }
  },
  methods: {
    n() {
      let numbers = [];

      // loop through numbers 1 to limit specified via input and push to array, start at 1 inclusive of limit
      for (var i = 1; i <= this.limit; i++) {
        numbers.push(i);
      }

      this.numbers = numbers;

      let sortedNumbers = [...numbers];

      for (let i = sortedNumbers.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1)); // get a random index from the unshuffled indexes
        const temp = sortedNumbers[i]; // store the current number
        // swap the two numbers
        sortedNumbers[i] = sortedNumbers[j];
        sortedNumbers[j] = temp;
      }

      return sortedNumbers;
    },
    hov(number) {
      // get all available divisible numbers, divide by 2 as potential divisors cannot be larger than the number divided by 2
      let nums = this.numbers.slice(1, number / 2);
      this.currentDivisors = [1, number];

      // // all numbers can be divided by 1
      document.getElementById("number-1").classList.add("active");

      // loop through array of potential divisble numbers and apply active class if remainder is 0
      for (let i = 0; i < nums.length; i++) {
        if (number % nums[i] === 0) {
          this.currentDivisors.push(nums[i]);
          document.getElementById(`number-${nums[i]}`).classList.add("active");
        }
      }
    },
    reset() {
      // loop through all current divisors and remove the active class from the elements
      for (let i = 0; i < this.currentDivisors.length; i++) {
        document
          .getElementById(`number-${this.currentDivisors[i]}`)
          .classList.remove("active");
      }
    }
  }
};
</script>

<style scoped>
.number {
  display: inline-block;
  padding: 5px;
  background-color: lightgrey;
  margin: 5px;
}

.active {
  background-color: red;
}
</style>
