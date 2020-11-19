<template>
  <div class="main">
    <h1>Simon The Game</h1>
    <div class="gameField">
      <button
        @click="handlerPlayerClick(0)"
        :class="{ active: btnActiveList[0] }"
      ></button>
      <button
        @click="handlerPlayerClick(1)"
        :class="{ active: btnActiveList[1] }"
      ></button>
      <button
        @click="handlerPlayerClick(2)"
        :class="{ active: btnActiveList[2] }"
      ></button>
      <button
        @click="handlerPlayerClick(3)"
        :class="{ active: btnActiveList[3] }"
      ></button>
    </div>
    <h2>
      Round: <span>{{ round }}</span>
    </h2>
    <p v-if="loseMessage" class="loseMessage">You Lose :(</p>
    <button class="btnStart" @click="gameStart()">Start</button>
    <h2>Difficulty level:</h2>
    <input
      type="radio"
      id="easy"
      :value="1500"
      v-model="difficulty"
      :disabled="round !== 0 && difficulty !== 1500"
    />
    <label for="easy">Easy</label>
    <input
      type="radio"
      id="normal"
      :value="1000"
      v-model="difficulty"
      :disabled="round !== 0 && difficulty !== 1000"
    />
    <label for="normal">Normal</label>
    <input
      type="radio"
      id="hard"
      :value="400"
      v-model="difficulty"
      :disabled="round !== 0 && difficulty !== 400"
    />
    <label for="hard">Hard</label>
  </div>
</template>

<script>
import sound1 from "../assets/sound/1.mp3";
import sound2 from "../assets/sound/2.mp3";
import sound3 from "../assets/sound/3.mp3";
import sound4 from "../assets/sound/4.mp3";

export default {
  data() {
    return {
      difficulty: 1000,
      round: 0,
      btnActiveList: {
        0: false,
        1: false,
        2: false,
        3: false,
      },
      btnSoundList: {
        0: sound1,
        1: sound2,
        2: sound3,
        3: sound4,
      },
      btnInRound: [],
      loseMessage: false,
    };
  },
  methods: {
    gameStart() {
      this.round = 0;
      this.loseMessage = false;
      this.btnInRound = [];
      this.gameCore();
    },
    getRandomBtn() {
      return Math.floor(Math.random() * 4);
    },
    gameCore() {
      this.round += 1;
      for (let i = 0; i < this.round; i++) {
        const activeBtn = this.getRandomBtn();
        this.btnInRound.push(activeBtn);
        setTimeout(() => this.activateBtn(activeBtn), this.difficulty * i);
      }
    },
    activateBtn(currentBtn) {
      setTimeout(() => (this.btnActiveList[currentBtn] = true), 100);
      this.playSound(currentBtn);
      setTimeout(() => this.$set(this.btnActiveList, currentBtn, false), 400);
    },
    handlerPlayerClick(value) {
      this.activateBtn(value);
      if (this.btnInRound.shift() !== value && this.round !== 0) {
        this.btnInRound = [];
        this.round = 0;
        this.loseMessage = true;
      } else if (this.btnInRound.length === 0 && this.round !== 0) {
        setTimeout(() => this.gameCore(), 1000);
      }
    },
    playSound(soundNumber) {
      const audio = new Audio(this.btnSoundList[soundNumber]);
      audio.play();
    },
  },
};
</script>

<style lang="scss">
.main {
  width: 540px;
  margin: 10% auto;
  h1 {
    text-align: center;
    margin-bottom: 50px;
  }
  .loseMessage {
    font-weight: bold;
  }
  .btnStart {
    background-color: #6dabe8;
    padding: 10px 40px;
    border-radius: 10px;
    cursor: pointer;
    &:hover {
      background-color: #78bcff;
    }
  }
  input,
  label {
    cursor: pointer;
    &:disabled,
    &:disabled + label {
      cursor: default;
    }
  }
  .gameField {
    width: 300px;
    height: 300px;
    background-color: #ffffff;
    margin: 0 auto;
    border-radius: 50%;
    float: left;
    margin-right: 40px;
    button {
      position: absolute;
      height: 300px;
      width: 300px;
      border-radius: 150px 150px 150px 150px;
      cursor: pointer;
      opacity: 0.6;
      &:hover {
        border: 4px solid black;
      }
      &:active,
      &:focus {
        outline: none;
      }
      &:nth-of-type(1) {
        background: #ff5643;
        clip: rect(0px, 300px, 150px, 150px);
        z-index: 1;
      }
      &:nth-of-type(2) {
        background: dodgerblue;
        clip: rect(0px, 150px, 150px, 0px);
        z-index: 1;
      }
      &:nth-of-type(3) {
        background: #feef33;
        clip: rect(150px, 150px, 300px, 0px);
        z-index: 1;
      }
      &:nth-of-type(4) {
        background: #bede15;
        clip: rect(150px, 300px, 300px, 150px);
        z-index: 1;
      }
    }
    .active {
      opacity: 1;
    }
  }
}
</style>