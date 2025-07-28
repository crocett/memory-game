<script setup>
import TurnOverCard from './components/TurnOverCard.vue';

import jesterImg from '../src/assets/jester.png';
import bardImg from '../src/assets/bard.png';
import herbalistImg from '../src/assets/herbalist.png';
import kingImg from '../src/assets/king.png';
import moonImg from '../src/assets/moon.png';
import nunImg from '../src/assets/nun.png';
import starImg from '../src/assets/star.png';
import visionaryImg from '../src/assets/visionary.png';

import { ref, onUnmounted, onMounted } from 'vue';

const images =[
  jesterImg,
  bardImg,
  herbalistImg,
  kingImg,
  moonImg,
  nunImg,
  starImg,
  visionaryImg
]

const restart = ref(false);

const cardImages = ref(
  [...images, ...images]
  .map((image, index) =>({
    id: index,
    image: image,
    isRotate: false,
    isMatched: false
  }))
  .sort(() => Math.random() - 0.5)
);

let firstCard = null;

const onCardFlip = (card) => {
  //если карту перевернули или есть совпадение
  if (card.isRotate || card.isMatched) return;
  //меняем флаг состояния переворота
  card.isRotate = true;
  //первый клик, запоминаем карту
  if (!firstCard) {
    firstCard = card;
    startTimer();
  }
  else{
    const prevCard = firstCard;
    firstCard = null;
    //при условии одинаковых картинок меняем флаги нахождения пары
    if(prevCard.image === card.image){
      card.isMatched = true;
      prevCard.isMatched = true;
          if (checkWin()) {
            setTimeout
            (() =>{
              clearInterval(timeWaste.value);
              timeWaste.value = null;
              firstClick.value = false;
              restart.value = true;
              alert(` Победа! Время: ${timer.value} секунд!`);
             },600) }

      return
    }

    else{
       setTimeout(() => {
        prevCard.isRotate = false;
        card.isRotate = false;
      }, 1000);
    }
    firstCard = null;
  }
}

const timer = ref(0);
const timeWaste = ref(null);
const firstClick = ref(false);

const startTimer = () => {
  if (firstClick.value || timeWaste.value) return;
  firstClick.value = true;

  timeWaste.value = setInterval(() => {
    timer.value++;
  }, 1000);
};

onUnmounted(() => {
  if (timeWaste.value) {
    clearInterval(timeWaste.value);
  }
});

const checkWin = () => {
  return cardImages.value.every(card => card.isMatched);
};

onMounted(() => {
  showAllCardsAtStart();

});


const showAllCardsAtStart = () => {
  cardImages.value.forEach(card =>{
    card.isRotate = true;
  })

  setTimeout(() => {
    cardImages.value.forEach(card =>{
      card.isRotate = false
    })
    }, 1500)

};

const resetGame = () => {
 
  timer.value = 0;
  timeWaste.value = null;
  firstClick.value = false;
  firstCard = null;
  restart.value = false; 


  cardImages.value = [...images, ...images]
    .map((image, index) => ({
      id: index,
      image,
      isRotate: false,
      isMatched: false
    }))
    .sort(() => Math.random() - 0.5);

    showAllCardsAtStart();

};


</script>

<template>

  <div class="gameboard">
      <div class="grid">
         <TurnOverCard v-for = "card in cardImages"
         :key="card.id"
         :image = "card.image"
         :is-rotate = "card.isRotate"
         :is-matched = "card.isMatched"

         @flip="() => onCardFlip(card)"
         >
        </TurnOverCard>

      </div>
      <button class="restart-button" v-if="restart" @click="resetGame">
        <p>НАЧАТЬ ЗАНОВО</p>
      </button>
  </div>
</template>

<style scoped>
@import './components/styles.css'
</style>