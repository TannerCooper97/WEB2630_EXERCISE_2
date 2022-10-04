<template>
  <div id="app">
    <section class="pContainer">
    <div class="player_container">
      <h1 class="container">PLAYER</h1>
      <div class="scoreBoard">
        <div class="scoreBoard container" style="background-color: green; margin: 0; color: white;" :style="{width: userScore + '0%'}">
          {{userScore}} {{endGame()}}

        </div>
      </div>
    </div>
    <div class="computer_container">
      <h1 class="container">COMPUTER</h1>
      <div class="scoreBoard">
        <div class="scoreBoard container" style="background-color: green; margin: 0; color: white;" :style="{width: computerScore + '0%'}">
          {{computerScore}} {{endGame()}}

        </div>
      </div>
    </div>
  </section>
  <section class="gameButtons" v-if = "visable">
    <div class="container">
      <button id="startGame" v-on:click="visable = !visable">NEW GAME</button>
    </div>
  </section>



  <section class="gameButtons" v-if="!visable">
    <div class="container">
      <button @click="Rock()" :class="playerChoice" id="Rock">👊</button>
      <button @click="Paper()" :class="playerChoice" id="Paper">✋</button>
      <button @click="Scissors()" :class="playerChoice" id="Scissors">✌️</button>
      <button @click="restart()" :class="playerChoice" id="Restart">RESTART</button>
    </div>
  </section>
  <section class="scoreMessageBox">
    <div class="container">
      <h2>Round: {{round}}</h2>
      <ul>
        <li v-for="item in turnResults" :key="{'playerTurn' : item.won, 'computerTurn' : item.lose, 'tied': item.tied}">
          {{item.message}}
        </li>
      </ul>
    </div>
  </section>
   
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      visable : true,
      playerChoice: {Rock: false, Paper: false, Scissors: false},
      computerScore: 0,
      userScore: 0,
      turnResults: [],
      round: 0,
    };

  },
  methods: {

    Rock: function () {
      var choiceSelected = this.computerRandomizer();
      //
      
     if (choiceSelected === 3) {
        this.turnResults.unshift({
            won: true,
            message: 'COMPUTER: ✌️ | 👊 BEATS ✌️ | PLAYER WINS!'
        });
        this.userScore += 1;
        this.round++;
      }
      else if (choiceSelected === 2) {
        this.turnResults.unshift({
            lose: true,
            message: 'COMPUTER: ✋ | ✋ BEATS 👊 | COMPUTER WINS!'
        });
        this.computerScore += 1;
        this.round++
      }
      else {
        this.turnResults.unshift({
            tied: true,
            message: 'COMPUTER: 👊 | YOU HAVE TIED!'
        });
        this.round++;
      }
    },

    Paper: function () {
      var choiceSelected = this.computerRandomizer();
      if (choiceSelected === 1) {
        this.turnResults.unshift({
            won: true,
            message: 'COMPUTER: 👊 | ✋ BEATS 👊 | PLAYER WINS!'
        });
        this.userScore += 1;
        this.round++;
      }
      else if (choiceSelected === 3) {
        this.turnResults.unshift({
            lose: true,
            message: 'COMPUTER: ✌️ | ✌️ BEATS ✋ | COMPUTER WINS!'
        });
        this.computerScore += 1;
        this.round++
      }
      else {
        this.turnResults.unshift({
            tied: true,
            message: 'COMPUTER: ✋ | YOU HAVE TIED!'
        });
        this.round++;
      }
    },

    Scissors: function () {
      var choiceSelected = this.computerRandomizer();
      if (choiceSelected === 2) {
        this.turnResults.unshift({
            won: true,
            message: 'COMPUTER: ✋ | ✌️ BEATS ✋ | PLAYER WINS!'
        });
        this.userScore += 1;
        this.round++;
      }
      else if (choiceSelected === 1) {
        this.turnResults.unshift({
            lose: true,
            message: 'COMPUTER: 👊 | 👊 BEATS ✌️ | COMPUTER WINS!'
        });
        this.computerScore += 1;
        this.round++
      }
      else {
        this.turnResults.unshift({
            tied: true,
            message: 'COMPUTER: ✌️ | YOU HAVE TIED!'
        });
        this.round++;
      }
    },

    restart: function () {
      this.visable = true
      this.playerChoice = {Rock: false, Paper: false, Scissors: false};
      this.computerScore = 0;
      this.userScore = 0;
      this.turnResults = [];
      this.round = 0;
    },

    computerRandomizer: function () {
      this.computerChoice = Math.floor((Math.random()*3) + 1);
      return this.computerChoice;
    },

    
    endGame: function() {
      if (this.userScore == 10) {
        setTimeout(() => {if(confirm('User wins! You won! Play Again?')) this.restart()}, 1)
      }
      else if (this.computerScore == 10) {
          setTimeout(() => {if(confirm('Computer wins! You lost! Play Again?')) this.restart()}, 1)
        }
      }
  },
  
  
};
</script>

<style>
 body{
    text-align: center;
    font-family: 'Roboto Condensed', sans-serif;
}

.pContainer{
    display: flex;
    align-items: center;
    justify-content: space-around;
    font-size: 1.5rem;
}

.player_container{
    width: 45%;
}

.computer_container{
    width: 45%;
}

.scoreBoard {
    width: 100%;
    color: black;
    height: 3rem;
    background-color: rgb(218, 218, 218);
    margin: auto;
    transition: width 1000ms;
    box-shadow: 0px 2px 4px black;
    
}

.gameButtons, .scoreMessageBox {
    margin-top: 30px;
    text-align: center;
    padding: 1rem;
    border: 1px solid black;
    box-shadow: 0px 2px 4px black;
}


.scoreMessageBox ul {
    list-style: none;
    font-weight: bold;
    text-transform: uppercase;
    display: inline;
}

.scoreMessageBox ul li {
    margin: 5px;
}

.container li item.won {
    color: green;
    background-color: rgb(148, 233, 148);
}

.container li item.lose {
    color: red;
    background-color: rgb(249, 143, 143);
}

.container li item.tied {
  color: blue;
  background-color: rgb(123, 123, 255);
}

.Turn {
    margin-top: 1em;
    margin-bottom: 1em;
    font-weight: bold;
    font-size: 1rem;
}

button {
    font-size: 30px;
    padding: 12px;
    box-shadow: 0 2px 2px black;
    margin: 20px;
    border-radius: 1rem;
    width: 20%;
}

#startGame {
    background-color: #ffffff;
}

#startGame:hover {
  background-color: #33a100;
  box-shadow: 0 3px 5px black;
}

#Rock {
    background-color: brown;
}

#Rock:hover {
    background-color:  rgba(165, 42, 42, 0.639);
    box-shadow: 0 3px 5px black;
}

#Paper {
    background-color: rgb(66, 66, 250);
}

#Paper:hover {
    background-color: rgb(125, 125, 255);
    box-shadow: 0 3px 5px black;
}

#Scissors {
    background-color: green;
}

#Scissors:hover {
    background-color: rgb(36, 181, 36);
    box-shadow: 0 3px 5px black;
}

#Restart {
    background-color: #ffffff;
}

#Restart:hover {
    background-color: rgb(94, 94, 94);
    box-shadow: 0 3px 5px black;
}
</style>
