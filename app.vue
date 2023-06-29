<template>
  <div class="text-center py-10">
    <h1 class="text-3xl font-bold mb-6">Famous Quotes of Steve Jobs</h1>
    <div class="text-xl mb-8">{{ currentQuote }}</div>
    <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded" @click="getRandomQuote">Next Quote</button>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';

export default {
  setup() {
    const quotes = ref([]);
    let currentQuote = ref("Loading quotes...")

    const fetchQuotes = async () => {
      const response = await fetch('/quotes.txt');
      const text = await response.text();
      quotes.value = text.split('\n').filter(line => line.trim() !== '');
    };

    const getRandomQuote = () => {
        if (quotes.value.length === 0) {
          currentQuote.value = 'Loading quotes...';
        } else {
          currentQuote.value = quotes.value[Math.floor(Math.random() * quotes.value.length)];
        }
    };

    onMounted(async () => {
      await fetchQuotes();
      getRandomQuote()
    });

    return {
      currentQuote,
      getRandomQuote
    };
  }
};
</script>

<style>
button {
  cursor: pointer;
}
</style>
