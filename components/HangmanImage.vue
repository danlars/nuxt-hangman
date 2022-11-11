<template>
  <div class="hangman">
    <div class="right-leg" :class="{ 'hide': wrongLettersCount < 8 }"/>
    <div class="left-leg" :class="{ 'hide': wrongLettersCount < 7 }"/>
    <div class="right-arm" :class="{ 'hide': wrongLettersCount < 6 }"/>
    <div class="left-arm" :class="{ 'hide': wrongLettersCount < 5 }"/>
    <div class="body" :class="{ 'hide': wrongLettersCount < 4 }"/>
    <div class="head" :class="{ 'hide': wrongLettersCount < 3 }"/>
    <div class="hang-vertical-bar" :class="{ 'hide': wrongLettersCount < 2 }"/>
    <div class="top-horizontal-bar" :class="{ 'hide': wrongLettersCount < 1 }"/>
    <div class="bar"/>
    <div class="stand"/>
  </div>
</template>
<script lang="ts" setup>
defineProps({
  wrongLettersCount: {
    type: Number,
    required: true,
  }
})
</script>
<style lang="scss" scoped>
.hangman {
  height: 500px;
  width: 400px;
  position: relative;
}

.stand {
  width: 200px;
  height: 10px;
  background-color: #000;
}

.bar {
  height: 480px;
  width: 10px;
  background-color: #000;
  margin-left: 95px;
}

.top-horizontal-bar {
  height: 10px;
  width: 200px;
  background-color: #000;
  margin-left: 95px;
}

.hang-vertical-bar {
  height: 50px;
  width: 10px;
  background-color: #000;
  position: absolute;
  left: 285px;
  top: 10px;
}

.head {
  border: 10px solid #000;
  height: 70px;
  width: 70px;
  position: absolute;
  left: 285px;
  top: 60px;
  border-radius: 50%;
  translate: calc(-50% + 5px);
  box-sizing: border-box;
}

.body {
  height: 100px;
  width: 10px;
  background-color: #000;
  position: absolute;
  left: 285px;
  top: 130px;
}

.left-arm {
  height: 10px;
  width: 70px;
  background-color: #000;
  position: absolute;
  left: 220px;
  top: 160px;
  rotate: 25deg;
  transform-origin: right;
}

.right-arm {
  height: 10px;
  width: 70px;
  background-color: #000;
  position: absolute;
  left: 290px;
  top: 160px;
  rotate: -25deg;
  transform-origin: left;
}

.left-leg {
  height: 10px;
  width: 90px;
  background-color: #000;
  position: absolute;
  left: 200px;
  top: 215px;
  rotate: -45deg;
  transform-origin: right;
}

.right-leg {
  height: 10px;
  width: 90px;
  background-color: #000;
  position: absolute;
  left: 290px;
  top: 215px;
  rotate: 45deg;
  transform-origin: left;
}

.hide {
  visibility: hidden;
}
</style>
