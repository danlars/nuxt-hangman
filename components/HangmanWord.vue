<template>
  <div class="hangman-word">
    <div class="key"
         :class="{'wrong': reveal && !guessedLetters.includes(character)}"
         v-for="(character, index) in word"
         :key="index">
      <span :class="{'hide': !guessedLetters.includes(character) && !reveal}">
        {{ character }}
      </span>
    </div>
  </div>
</template>
<script lang="ts" setup>
import {computed, PropType} from 'vue'

const props = defineProps({
  word: {
    type: String,
    required: true,
  },
  guessedLetters: {
    type: Array as PropType<string[]>,
    required: false,
    default: () => [],
  },
  reveal: {
    required: false,
    type: Boolean,
    default: () => false,
  }
});
</script>
<style lang="scss" scoped>
.hangman-word {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 1rem;
}

.key {
  font-size: 3rem;
  border-bottom: solid .3rem currentColor;
  width: 3rem;
  display: flex;
  justify-content: center;
  font-weight: bold;
  text-transform: uppercase;
}

.wrong {
  color: red;
}

.hide {
  visibility: hidden;
}
</style>
