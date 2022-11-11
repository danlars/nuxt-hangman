<template>
  <div class="container">
    <div class="title">
      <span v-if="gameState === HangmanGameStateEnum.lost">
        You lost! <button type="button" @click="restart">Try again </button>
      </span>
      <span v-else-if="gameState === HangmanGameStateEnum.won">
        You win! <button type="button" @click="restart">Try again </button>
      </span>
    </div>
    <HangmanImage :wrong-letters-count="wrongLettersCount" />
    <HangmanWord :word="word" :guessed-letters="guessedLetters" :reveal="gameState !== HangmanGameStateEnum.playing" />
    <VirtualKeyboard :guessed-letters="guessedLetters" :word="word" />
  </div>
</template>
<script lang="ts" setup>
import {onMounted, provide, Ref, ref} from 'vue';
import {HangmanGameStateEnum} from '~/enums/hangman-game-state.enum';
import {VirtualKeyboardProvider} from '~/constants/VirtualKeyboard.provider';
import words from './mocks/words.json';

const getNewWord = () => words[Math.floor(Math.random() * words.length)];
const word = ref('') as Ref<string>;
const gameState = ref(HangmanGameStateEnum.playing) as Ref<HangmanGameStateEnum>;
const guessedLetters = ref([]) as Ref<string[]>;
const wrongLettersCount = ref(0) as Ref<number>;
const addGuessedLetter = (letter: string) => {
  if (guessedLetters.value.includes(letter) || gameState.value !== HangmanGameStateEnum.playing) {
    return;
  }
  const newGuessedLetters = [
      ...guessedLetters.value,
      letter,
  ];
  const wrongLettersCounter = newGuessedLetters.filter(guessedLetters => !word.value.includes(guessedLetters)).length;
  const correctLettersCount = word.value.split('').filter(character => newGuessedLetters.includes(character)).length;
  if (correctLettersCount === word.value.length) {
    gameState.value = HangmanGameStateEnum.won;
  } else if (wrongLettersCounter >= 8) {
    gameState.value = HangmanGameStateEnum.lost;
  }

  wrongLettersCount.value = wrongLettersCounter;
  guessedLetters.value = newGuessedLetters;
};
const restart = () => {
  gameState.value = HangmanGameStateEnum.playing;
  guessedLetters.value = [];
  wrongLettersCount.value = 0;
  word.value = getNewWord();
};
provide(VirtualKeyboardProvider.onKeyPress, addGuessedLetter);
onMounted(() => {
  word.value = getNewWord();
});
</script>
<style lang="scss" scoped>
.container {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  align-items: center;
}

.title {
  text-align: center;
  height: 4.5rem;
  font-size: 3rem;
}
</style>
