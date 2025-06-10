<script setup>
  import { ref } from 'vue';
  import axios from 'axios';

  const isLoading = ref(null);
  const quote = ref(null);
  
  async function generate() {
    let quote_response;
    try {
      isLoading.value = true;
      quote_response = await axios.get('https://thequoteshub.com/api/random-quote?format=json');
    } catch (err) {
      alert(err);
    } finally {
      isLoading.value = false;
    }
    quote.value = {
      text: quote_response.data.text,
      author: quote_response.data.author
    }
  }
</script>

<template>
  <main>
    <section v-if="isLoading">
      <img src="./assets/loading.gif" width="100px" height="100px">
    </section>
    <section v-else>
      <span v-if="quote != null" class="quote">"{{ quote.text }}"</span>
      <span v-else class="quote">Click the button below to generate a quote</span>
      <span v-if="quote != null" class="author">{{ quote.author }}</span>
      <span v-else class="author">Random Quote Generator | © Rovic Aliman 2025</span>
      <button @click="generate">{{ quote == null ? "Generate" : "Generate another quote" }}</button>
    </section>
  </main>
</template>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100..900;1,100..900&display=swap');

  :root {
    --main-padding: 24px;
  }

  body {
    margin: 0;
    font-family: "Roboto";
    color: white;
    box-sizing: border-box;
    list-style: none;
  }

  main {
    background: linear-gradient(#000000AB,#000000AB), url("https://picsum.photos/1920/1080");
    background-repeat: no-repeat;
    background-size: cover;
    height: calc(100vh - var(--main-padding) * 2);
    width: calc(100vw - var(--main-padding) * 2);
    display: flex;
    justify-content: center;
    align-items: center;
    padding: var(--main-padding);
  }

  section {
    display: flex;
    flex-direction: column;
    justify-content: center;
    gap: 10px;
  }

  button {
    padding: 8px 16px;
    border-radius: 8px;
    width: max-content;
    place-self: center;
  }

  button:active {
    background-color: #AAA;
  }

  .quote {
    font-size: 24px;
    text-align: center;
    font-style: italic;
    max-height: 300px;
    overflow-y: auto;
  }

  .author {
    font-size: 16px;
    text-align: center;
  }
</style>