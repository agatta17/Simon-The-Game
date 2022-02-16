<template>
  <div class="simon">
    <h1 class="title">Simon Says</h1>
    <div class="game">
      <div class="buttons" :class="{'buttons_active': playerTurn}">
        <div class="button blue" :class="{'button_highlighted': buttonId === 1}" @click="takeTurn(1)"></div>
        <div class="button red" :class="{'button_highlighted': buttonId === 2}" @click="takeTurn(2)"></div>
        <div class="button yellow" :class="{'button_highlighted': buttonId === 3}" @click="takeTurn(3)"></div>
        <div class="button green" :class="{'button_highlighted': buttonId === 4}" @click="takeTurn(4)"></div>
      </div>
      <div class="info">
        <div class="round">Раунд: {{round}}</div>
        <button class="start" @click="start" :disabled="round > 0 && !playerTurn">Начать</button>
        <div class="message" :class="{'message_hidden': !gameOver}">Игра окончена!<br/>Вы выбыли после {{reachedRound}} раунда.</div>
        <div>
          <span class="level-title">Уровень сложности</span>
          <ul class="level">
            <li>
              <input type="radio" name="level" value="light" id="light" v-model="level">
              <label for="light">Легкий</label>
            </li>
            <li>
              <input type="radio" name="level" value="medium" id="medium" v-model="level">
              <label for="medium">Средний</label>
            </li>
            <li>
              <input type="radio" name="level" value="hard" id="hard" v-model="level">
              <label for="hard">Сложный</label>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import sound1 from '../assets/1.ogg';
import sound2 from '../assets/2.ogg';
import sound3 from '../assets/3.ogg';
import sound4 from '../assets/4.ogg';

export default {
  name: 'Simon',

  data() {
    return {
      round: 0,
      level: 'light',
      playNumber: 0,
      buttonId: 0,
      sequence: [],
      playerTurn: false,
      turnNumber: 0,
      gameOver: false,
      reachedRound: 0,
      soundList: {
        1: sound1,
        2: sound2,
        3: sound3,
        4: sound4,
      },
      audio: new Audio(),

      levelsConfig: {
        light: 1500,
        medium: 1000,
        hard: 400,
      }
    }
  },

  methods: {
    randomInteger(min, max) {
      let rand = min - 0.5 + Math.random() * (max - min + 1);
      return Math.round(rand);
    },

    start() {
      this.round = 1;
      this.gameOver = false;
      this.finishRound();
      this.play();
    },

    play() {
      this.playNumber++;
      this.buttonId = this.randomInteger(1,4);
      this.sequence.push(this.buttonId);
      this.audio.src = this.soundList[this.buttonId];
      this.audio.play();
      setTimeout( ()=> this.buttonId = 0, 300)
      setTimeout( ()=> {
        if (this.playNumber < this.round) {
          this.play()
        } else {
          this.playNumber = 0;
          this.playerTurn = true;
        }
      }, this.levelsConfig[this.level]);
    },

    takeTurn(btnId) {
      this.audio.src = this.soundList[btnId];
      this.audio.play();
      this.checkTurn(btnId)
    },

    checkTurn(btnId) {
      if (btnId === this.sequence[this.turnNumber]) {
        this.turnNumber++;
        if (this.turnNumber === this.sequence.length) {
          this.finishRound();
          setTimeout(()=>this.round++, 1000);
        }
      } else {
        this.gameOver = true;
        this.reachedRound = this.round;
        this.round = 0;
        this.playerTurn = false;
      }
    },

    finishRound() {
      this.playerTurn = false;
      this.turnNumber = 0;
      this.sequence = [];
    }
  },

  watch: {
    round(value) {
      if (value > 1) {
        this.play()
      }
    }
  },
}
</script>

<style lang="scss">
.title {
  text-align: center;
  margin-bottom: 50px;
}

.game {
  margin: 0 auto;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
}

.buttons {
  width: 300px;
  height: 300;
  box-shadow: 2px 1px 12px #aaa;
  border-radius: 150px 150px 150px 150px;
  display: grid;
  grid-template-columns: 150px 150px;
  padding: 2px;
  pointer-events: none;
  &_active {
    cursor: pointer;
    pointer-events: auto;
  }
  
  .button {
    width: 150px;
    height: 150px;
    box-sizing: border-box;
    opacity: .6;
    -webkit-tap-highlight-color: rgba(255, 255, 255, 0);
    -webkit-tap-highlight-color: transparent;
    &_highlighted {
      opacity: 1;
    }
  }

  .blue {
    border-radius: 150px 0px 0px 0px;
    background: dodgerblue;
    &:hover {
      border-left: solid 3px #2c3e50;
      border-top: solid 3px #2c3e50;
    }
  }

  .red {
    border-radius: 0px 150px 0px 0px;
    background: #FF5643;
    &:hover {
      border-right: solid 3px #2c3e50;
      border-top: solid 3px #2c3e50;
    }
  }

  .yellow {
    border-radius: 0px 0px 0px 150px;
    background: #FEEF33;
    &:hover {
      border-left: solid 3px #2c3e50;
      border-bottom: solid 3px #2c3e50;
    }
  }

  .green {
    border-radius: 0px 0px 150px 0px;
    background: #BEDE15;
    &:hover {
      border-right: solid 3px #2c3e50;
      border-bottom: solid 3px #2c3e50;
    }
  }
}

.info {
  padding: 35px 0 35px 35px;
  min-height: 290px;

  .round {
    font-size: 1.5em;
    font-weight: 600;
    margin-bottom: 15px;
  }

  .start {
    width: 5em;
    box-sizing: border-box;
    font-size: 1.4em;
    -webkit-border-radius: 10px 10px 10px 10px;
    border-radius: 10px 10px 10px 10px;
    background: #6DABE8;
    border: none;
    padding: 0.3em 0.6em;
    cursor: pointer;
    margin-bottom: 15px;
    -webkit-tap-highlight-color: rgba(255, 255, 255, 0);
    -webkit-tap-highlight-color: transparent;
    &:hover {
      opacity: .8;
    }
    &:active {
      transform: scale(.9);
    }
  }

  .message {
    padding-bottom: 15px;
    color: #FF5643;
    
    &_hidden {
      visibility: hidden;
    }
  }

  .level-title {
    font-size: 1.5em;
    font-weight: 600;
  }

  .level {
    list-style-type: none;
    padding-inline-start: 0;
  }
}
</style>
