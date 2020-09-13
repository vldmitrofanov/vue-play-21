<template>
  <div class="container">
    <div class="score-panel">
      <div class="score-title">
        <h1 class="title">vue-play-21</h1>
      </div>
      <div class="score-table flex-center">
        <div class="total-left">
          <label>Player</label>
          <span>{{ playerScoreTotal }}</span>
        </div>
        <div class="total-right">
          <label>Dealer</label>
          <span>{{ dealerScoreTotal }}</span>
        </div>
      </div>
    </div>

    <div class="row cards21 dealer-table">
      <ul class="deck">
        <li
          v-for="(card, index) in dealerCards"
          :key="index"
          :class="[gameRunning ? 'back' : card.class]"
          class="card21"
        />
      </ul>
      <div class="dealer-score score" v-if="!gameRunning">
        {{ dealerScore }}
      </div>
      <div class="dealer-score score" v-else>??</div>
    </div>

    <div class="row cards21 user-table">
      <ul class="deck">
        <li
          v-for="(card, index) in playerCards"
          :key="index"
          :class="card.class"
          class="card21"
        />
      </ul>

      <div class="draw-buttons" v-if="gameRunning">
        <button type="button" class="btn btn-light" @click="drawCard">
          Draw
        </button>
        <button type="button" class="btn btn-info" @click="playerPass">
          Stand
        </button>
      </div>
      <div class="draw-buttons" v-else>
        <button class="btn btn-primary" type="button" @click="startNewGame">
          New Game
        </button>
      </div>
      <div class="player-score score">{{ playerScore }}</div>
    </div>

    <div class="row cards21" v-if="false">
      <ul class="deck">
        <li class="card21 back"></li>
        <li
          v-for="(card, index) in deck"
          :key="index"
          :class="card.class"
          class="card21"
        />
      </ul>
    </div>

    <div class="row flex-center mt-5" style="justify-content: center">
      <a
        href="https://github.com/vldmitrofanov/vue-play-21"
        style="color: #888; font-size: 14px; display: flex; align-items: center"
        ><img
          src="/images/GitHub-32px.png"
          style="height: 20px; margin-right: 6px"
        />
        https://github.com/vldmitrofanov/vue-play-21</a
      >
    </div>
  </div>
</template>

<script>
import Deck from "~/mixins/carddeck.js";
export default {
  mixins: [Deck],
  data() {
    return {
      deck: [],
      playerCards: [],
      dealerCards: [],
      pass: false,
      gameRunning: false,
      playerScoreTotal: 0,
      dealerScoreTotal: 0,
    };
  },
  computed: {
    playerScore() {
      let score = 0;
      const len = this.playerCards.length;
      for (let i = 0; i < len; i++) {
        score += this.playerCards[i].value;
      }
      return score;
    },
    dealerScore() {
      let score = 0;
      const len = this.dealerCards.length;
      for (let i = 0; i < len; i++) {
        score += this.dealerCards[i].value;
      }
      return score;
    },
  },
  mounted() {
    this.newDeck();
  },
  methods: {
    shuffle(array) {
      //array.sort(() => Math.random() - 0.5);
      var currentIndex = array.length,
        temporaryValue,
        randomIndex;

      // While there remain elements to shuffle...
      while (0 !== currentIndex) {
        // Pick a remaining element...
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex -= 1;

        // And swap it with the current element.
        temporaryValue = array[currentIndex];
        array[currentIndex] = array[randomIndex];
        array[randomIndex] = temporaryValue;
      }

      return array;
    },
    newDeck() {
      this.deck = [];
      this.deck = [...this.deck0];
      this.shuffle(this.deck);
    },
    startNewGame() {
      this.pass = false;
      this.gameRunning = true;
      this.newDeck();
      this.playerCards = [];
      this.dealerCards = [];
      this.playerGetCard();
      this.playerGetCard();
      this.dealerGetCard();
      this.dealerGetCard();
    },
    playerGetCard() {
      this.playerCards = [...this.playerCards, this.deck[0]];
      this.deck.shift();
    },
    dealerGetCard() {
      this.dealerCards = [...this.dealerCards, this.deck[0]];
      this.deck.shift();
    },
    checkOverDraw() {
      if (this.playerScore > 21 || this.dealerScore > 21) {
        this.getWinner();
      }
    },
    playerWin() {
      //alert("You won!");
      this.$swal({
        icon: "success",
        title: "YOU WON!",
        text: "Congratulations!",
      });
      this.playerScoreTotal++;
    },
    playerLost(text) {
      this.$swal({
        icon: "error",
        title: "You lost",
        text: text,
      });
      //alert("You lost!");
      this.dealerScoreTotal++;
    },
    getWinner() {
      this.gameRunning = false;
      if (this.playerScore > 21) {
        this.playerLost("You busted");
        return;
      }
      if (this.dealerScore > 21) {
        this.playerWin();
        return;
      }
      if (this.dealerScore > this.playerScore) {
        this.playerLost();
        return;
      } else if (this.dealerScore < this.playerScore) {
        this.playerWin();
        return;
      } else {
        //alert("You're even");
        this.$swal({
          icon: "info",
          title: "You're even",
          text: "no winners!",
        });
      }
    },
    dealerTurn() {
      if (!this.gameRunning) {
        return false;
      }
      if (this.dealerScore <= 16) {
        this.dealerGetCard();
      }
      if (this.pass) {
        this.getWinner();
      }
    },
    drawCard() {
      if (!this.gameRunning) {
        return false;
      }
      this.playerGetCard();
      this.checkOverDraw();
      this.dealerTurn();
    },
    playerPass() {
      this.pass = true;
      this.dealerTurn();
    },
  },
};
</script>
