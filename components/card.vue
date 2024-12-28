<template>
  <div id="flashcard-app" class="container">
    <h1>家事スケジュール</h1>
    <div class="flashcard-form">
      <label for="back">
        Task
        <input v-on:keypress.enter="addNew" v-model="newBack" type="text" id="back" />
      </label>
      <button v-on:click="addNew">Add a New Card</button>
    </div>

    <v-chip-group>
      <v-chip v-for = "(card, index) in cards" :key="index" :title="'Item ' + card.back" @click="removeCard(index)">
        {{ card.back }}
      </v-chip>
    </v-chip-group>

    <ul class="flashcard-list">
      <li v-for="(card, index) in shuffleCards"
        :key="index" 
        @click="toggleCard(card)">
        <transition name="flip" @after-enter="onTransitionEnd">
          <p :key="card.flipped" class="card">
            {{ card.flipped ? card.back : card.front }}
            <span v-on:click="cards.splice(index, 1)" class="delete-card">X</span>
          </p>
        </transition>
      </li>
    </ul>
  </div>
</template>

<style>
body {
  font-family: 'Montserrat', sans-serif;
  text-align: center;
}

ul {
  padding-left: 0;
  display: flex;
  flex-flow: row wrap;
}

li {
  list-style-type: none;
  padding: 10px 10px;
  transition: all 0.3s ease;
}

.container {
  max-width: 100%;
  padding: 2em;
}

.card {
  display: block;
  width: 150px;
  height: 175px;
  padding: 80px 50px;
  background-color: #76757d;
  border-radius: 7px;
  margin: 5px;
  text-align: center;
  line-height: 27px;
  cursor: pointer;
  position: relative;
  color: #fff;
  font-weight: 600;
  font-size: 20px;
  box-shadow: 9px 10px 22px -8px rgba(209, 193, 209, 0.5);
  will-change: transform;
}

li:hover {
  transform: scale(1.1);
}

/* li:nth-child(-n + 3) .card {
  background-color: #e65f51;
}

li:nth-child(2n + 1) .card {
  background-color: #a17de9;
}

li:nth-child(4n) .card {
  background-color: #feca34;
}

li:nth-child(5n - 2) .card {
  background-color: #51aae5;
}

li:nth-child(4n + 4) .card {
  background-color: #feca34;
}

li:nth-child(-7n + 7) .card {
  background-color: #e46055;
} */

.delete-card {
  position: absolute;
  right: 0;
  top: 0;
  padding: 10px 15px;
  opacity: 0.4;
  transition: all 0.5s ease;
}

.delete-card:hover,
.error {
  opacity: 1;
  transform: rotate(360deg);
}

.flip-enter-active {
  transition: all 0.4s ease;
}

.flip-leave-active {
  display: none;
}

.flip-enter-to,
.flip-leave-to {
  transform: rotateY(180deg);
  opacity: 0;
}

/* Form */
.flashcard-form {
  position: relative;
}

label {
  font-weight: 400;
  color: #333;
  margin-right: 10px;
}

input {
  border-radius: 5px;
  border: 2px solid #eaeaea;
  padding: 10px;
  outline: none;
}

button {
  border-radius: 5px;
  border: 1px solid #87cb84;
  background-color: #87cb84;
  padding: 8px 15px;
  outline: none;
  font-size: 14px;
  font-weight: 700;
  color: #fff;
  cursor: pointer;
  transition: all 0.3s ease;
}

button:hover {
  background-color: #70a66f;
}

.error {
  margin-top: 10px;
  display: block;
  color: #e44e42;
  font-weight: 600;
}

.player1 {
  background-color: #e65f51 !important;
}

.player2 {
  background-color: #70a66f !important;
}

</style>

<script setup>
import { ref } from 'vue';

const cards = ref([]);

const newBack = ref('');
const count = ref(0);

const addNew = () => {
  if (newBack.value) {
    cards.value.push({ front: '', back: newBack.value, flipped: false });
    newBack.value = '';
  } 
};

const shuffleCards = computed(() => {
  return cards.value.sort(() => Math.random() - 0.5);
});

const removeCard = (index) => {
  cards.value.splice(index, 1);
};

const toggleCard = (card) => {
  if (!card.flipped) {
    card.flipped = !card.flipped;
    count.value += 1;
  }
};

const onTransitionEnd = (el) => {
  const clickedCard = el
  console.log(clickedCard);
  clickedCard.classList.add('player' + (count.value % 2 + 1));
};
</script>
