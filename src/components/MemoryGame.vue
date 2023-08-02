<template>
    <div class="memory-game">
      <div v-for="(card, index) in shuffledCards" :key="index" @click="flipCard(index)">
        <div v-if="card.flipped" class="card">{{ card.value }}</div>
        <div v-else class="card back"></div>
      </div>
      <button @click="resetGame">Restart Game</button>
      <div v-if="isGameFinished">
      Congratulations! You have completed the game.
    </div>
    </div>

  </template>
  
  <script setup>
  import { ref, onMounted, computed } from 'vue';
  import { shuffle } from 'lodash';
  const cards = ref([
    { value: 'A', flipped: false, matched: false },
    { value: 'B', flipped: false, matched: false },
    { value: 'A', flipped: false, matched: false },
    { value: 'B', flipped: false, matched: false },

    { value: 'C', flipped: false, matched: false },
    { value: 'D', flipped: false, matched: false },
    { value: 'C', flipped: false, matched: false },
    { value: 'D', flipped: false, matched: false },


    { value: 'E', flipped: false, matched: false },
    { value: 'F', flipped: false, matched: false },
    { value: 'E', flipped: false, matched: false },
    { value: 'F', flipped: false, matched: false },
  ]);
  
  const shuffledCards = ref([]);
  
  onMounted(() => {
    shuffleCards();
  });
  
  const shuffleCards = () => {
    // shuffledCards.value = [...cards.value].sort(() => Math.random() - 0.5);
    shuffledCards.value = shuffle([...cards.value]); 
  };
  
  const flipCard = (index) => {
    if (!shuffledCards.value[index].flipped) {
      shuffledCards.value[index].flipped = true;
  
      const flippedCards = shuffledCards.value.filter((card) => card.flipped && !card.matched);
  
      if (flippedCards.length === 2) {
        if (flippedCards[0].value === flippedCards[1].value) {
          flippedCards.forEach((card) => (card.matched = true));
        } else {
          setTimeout(() => {
            flippedCards.forEach((card) => (card.flipped = false));
          }, 500);
        }
      }
    }
  };


  ////////////////////////////resetGame//////////////////////////

  const resetGame = () => {
  shuffledCards.value.forEach((card) => {
    card.flipped = false;
    card.matched = false;
  });
  shuffleCards();
};

  ////////////////////////////isGameFinished//////////////////////////

const isGameFinished = computed(() => {
  return shuffledCards.value.every((card) => card.matched);
});

  </script>
  
  <style>

  body{
    display: flex;
    justify-content: center;
  }
  .memory-game {
    display: grid;
    grid-template-columns: repeat(4, 100px);
    grid-gap: 10px;
    margin-top: 50px;;
  }
  
  .card {
    width: 100px;
    height: 100px;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: lightblue;
    cursor: pointer;
    font-size: 24px;
  }
  
  .back {
    background-color: gray;
  }
  
  </style>