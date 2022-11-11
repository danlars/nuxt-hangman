<template>
  <div class="virtual-keyboard">
    <button class="key"
            :disabled="guessedLetters.includes(key)"
            :class="{ matched: guessedLetters.includes(key) && word.includes(key)}"
            @click="onKeyPress(key)"
            v-for="key in keys"
            :key="key">
      {{ key }}
    </button>
  </div>
</template>
<script lang="ts" setup>
import {inject, onBeforeUnmount, onMounted, PropType} from 'vue';
import {VirtualKeyboardProvider} from '~/constants/VirtualKeyboard.provider';
defineProps({
  guessedLetters: {
    type: Array as PropType<string[]>,
    required: true,
  },
  word: {
    type: String,
    required: true,
  }
});
const keys = [
  'q',
  'w',
  'e',
  'r',
  't',
  'y',
  'u',
  'i',
  'o',
  'p',
  'a',
  's',
  'd',
  'f',
  'g',
  'h',
  'j',
  'k',
  'l',
  'z',
  'x',
  'c',
  'v',
  'b',
  'n',
  'm',
];
const addLetter = inject(VirtualKeyboardProvider.onKeyPress) as (letter: string) => void;
const onKeyPress = (letter: string) => addLetter(letter);
const handler = (event: KeyboardEvent) => {
  const key = event.key.toLowerCase();
  const isCtrlPressed = event.ctrlKey || event.metaKey;
  if (isCtrlPressed || !keys.includes(key)) {
    return;
  }
  event.preventDefault();
  addLetter(key);
};
onMounted(() => {
  document.addEventListener('keypress', handler);
});
onBeforeUnmount(() => {
  document.removeEventListener('keypress', handler);
})
</script>

<style lang="scss" scoped>
.virtual-keyboard {
  --background-color: transparent;
  --color: #000;
  width: 100%;
  display: grid;
  gap: .75rem;
  grid-template-columns: repeat(auto-fit, minmax(50px, 65px));
}

.key {
  border: solid 2px #000000;
  background: var(--background-color);
  padding: .8rem 1rem;
  color: var(--color);
  font-size: 1.5rem;
  font-weight: bold;
  text-transform: uppercase;
  cursor: pointer;

  &:hover:not(:disabled), &:focus:not(:disabled) {
    --background-color: hsl(199, 97%, 75%);
    --color: #fff;
  }

  &:disabled {
    opacity: .5;
    cursor: not-allowed;

    &.matched {
      --background-color: hsl(199, 97%, 75%);
      --color: #fff;
      opacity: 1;
    }
  }
}
</style>
