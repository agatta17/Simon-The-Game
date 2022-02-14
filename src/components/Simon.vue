<template>
  <div class="simon">
    <h1 class="title">Simon Says</h1>
    <div class="game">
      <div class="buttons">
        <div class="button blue"></div>
        <div class="button red"></div>
        <div class="button yellow"></div>
        <div class="button green"></div>
      </div>
      <div class="info">
        <div class="round">Раунд: 0</div>
        <button class="start">Начать</button>
        <div class="message">Игра окончена!</div>
        <div>
          <span class="level-title">Уровень сложности</span>
          <ul class="level">
            <li>
              <input type="radio" name="level" value="light" id="light" checked>
              <label for="light">Легкий</label>
            </li>
            <li>
              <input type="radio" name="level" value="medium" id="medium">
              <label for="medium">Средний</label>
            </li>
            <li>
              <input type="radio" name="level" value="hard" id="hard">
              <label for="hard">Сложный</label>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Simon',
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

  .button {
    width: 150px;
    height: 150px;
    box-sizing: border-box;
    cursor: pointer;
    opacity: .6;
    -webkit-tap-highlight-color: rgba(255, 255, 255, 0);
    -webkit-tap-highlight-color: transparent;
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
  }

  .message {
    padding-bottom: 15px;
    color: #FF5643
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
