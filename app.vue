<template>
  <div>
    <h1 class="headline">Famous Quotes of Steve Jobs</h1>
    <div class="quote">{{ currentQuote }}</div>
    <button @click="fetchRandomQuote" class="button">Get Another Quote</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      quotes: [],
      currentQuote: ''
    };
  },
  mounted() {
    this.fetchQuotes();
  },
  methods: {
    fetchQuotes() {
      fetch('/quotes.txt')
          .then(response => response.text())
          .then(data => {
            this.quotes = data.split('\n').filter(quote => quote !== '');
            this.fetchRandomQuote();
          });
    },
    fetchRandomQuote() {
      const randomIndex = Math.floor(Math.random() * this.quotes.length);
      const newQuote = this.quotes[randomIndex];

      if (newQuote === this.currentQuote) {
        // If the new quote is the same as the current quote, recursively call the method again
        this.fetchRandomQuote();
      } else {
        this.currentQuote = newQuote;
      }
    }
  }
};
</script>

<style>
.headline {
  text-align: center;
  margin-bottom: 20px;
}

.quote {
  text-align: center;
  font-size: 24px;
  margin-bottom: 20px;
}

.button {
  display: block;
  margin: 0 auto;
}
</style>
