<template>
  <div class="container">
    <div class="row">
      <div class="col-sm-4 col-xs-12">
        <h1>Rules</h1>
        <Rules />
      </div>
      <div class="col-sm-4 col-xs-12">
        <div class="score">
          <div id="userLabel" class="badge">Player</div>
          <div id="compLabel" class="badge">Comp</div>
          <span id="userScore">{{ userWins }}</span>
          :
          <span id="compScore">{{ computerWins }}</span>
        </div>
        <div class="radioWrapper">
          <RadioButtons @radioValueUpdated="updateRadioValue" />
        </div>
      </div>
      <div class="col-sm-4 col-xs-12">
        <h1>Last moves</h1>
        <ul class="lastMoves">
          <li class="lastMove" v-for="(lastMove, index) in showingLastMoves" :key="index">
            {{ lastMove.player }} vs {{ lastMove.computer }}
          </li>
        </ul>
      </div>
    </div>
    <div class="row">
      <div class="col-xs-12">
        <div class="wrapper">
          <span class="choices">
            {{ playerChoice }}
          </span>
          <span class="message">
            {{ message }}
          </span>
          <span class="choices">
            {{ computersChoice }}
          </span>
        </div>
      </div>
      <div class="col-xs-12 col-sm-8 col-sm-offset-2 ">
        <ul class="emojis">
          <li
            class="emoji"
            v-for="(emoji, index) in emojis"
            v-bind:key="emoji.name"
            @click="pretendProcessing(emoji, index)"
          >
            {{ emoji.symbol }}
            <span class="name">{{ emoji.name }}</span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Rules from '../rules/Rules.vue';
import RadioButtons from '../radioButtons/RadioButtons.vue';

type Emoji = {
  name: string;
  symbol: string;
  strength: number;
  occurr: number;
  weakness: number[];
};

type Data = {
  emojis: Emoji[];
  message: string;
  computerWins: number;
  userWins: number;
  playerChoice: string;
  computersChoice: string;
  lastMoves: {
    player: string;
    computer: string;
  }[];
  radioValue: string;
  computerGuess: number;
};

export default defineComponent({
  name: 'Game',
  components: {
    Rules,
    RadioButtons,
  },
  data(): Data {
    return {
      emojis: [
        {
          name: 'Rock',
          symbol: '👊',
          strength: 2,
          occurr: 0,
          weakness: [1, 4],
        },
        {
          name: 'Paper',
          symbol: '✋',
          strength: 1,
          occurr: 0,
          weakness: [2, 3],
        },
        {
          name: 'Scissors',
          symbol: '✌️',
          strength: 0,
          occurr: 0,
          weakness: [0, 4],
        },
        {
          name: 'Lizard',
          symbol: '🦎',
          strength: 3,
          occurr: 0,
          weakness: [0, 2],
        },
        {
          name: 'Spock',
          symbol: '🖖',
          strength: 4,
          occurr: 0,
          weakness: [1, 3],
        },
      ],
      playerChoice: '',
      computersChoice: '',
      message: "Let's play!",
      computerWins: 0,
      userWins: 0,
      lastMoves: [],
      radioValue: '',
      computerGuess: 0,
    };
  },
  methods: {
    clickIt(emoji: Emoji, index: number) {
      const randomIndex = Math.floor(Math.random() * this.emojis.length);
      const randomWeakness = this.emojis[this.indexOfMaxOccur()].weakness[this.randomOfTwo()];
      const calculatedIndex = [randomIndex, randomWeakness][this.randomOfTwo()];
      if (this.radioValue === 'Random') {
        this.setComputerChoice(randomIndex);
      } else if (this.radioValue === 'Counter most used') {
        this.setComputerChoice(randomWeakness);
      } else {
        this.setComputerChoice(calculatedIndex);
      }
      this.emojis[index].occurr += 1;
      console.log(
        this.emojis.map((ele) => ele.occurr),
        this.indexOfMaxOccur(),
        this.emojis[this.indexOfMaxOccur()].weakness,
        randomWeakness,
        this.computerGuess,
      );
      const nums = [-1, -3, 2, 4];
      if (emoji.strength === this.computerGuess) {
        this.message = 'It is a tie';
      } else if (nums.indexOf(emoji.strength - this.computerGuess) !== -1) {
        this.message = 'wins';
        this.userWins += 1;
      } else {
        this.message = 'loses to';
        this.computerWins += 1;
      }
      this.lastMoves.push({ player: this.playerChoice, computer: this.computersChoice });
    },
    pretendProcessing(emoji: Emoji, index: number) {
      this.playerChoice = emoji.symbol;
      this.message = 'fighting';
      let x = 0;
      const intervalID = setInterval(() => {
        this.computersChoice = this.emojis[Math.floor(Math.random() * this.emojis.length)].symbol;
        if (x === 10) {
          window.clearInterval(intervalID);
          this.clickIt(emoji, index);
        }
        x += 1;
      }, 100);
    },
    indexOfMaxOccur() {
      const arrayOfOccurrences = this.emojis.map((ele) => ele.occurr);
      let max = arrayOfOccurrences[0];
      let maxIndex = 0;
      arrayOfOccurrences.forEach((item, index) => {
        if (item > max) {
          maxIndex = index;
          max = item;
        }
      });
      return maxIndex;
    },
    randomOfTwo() {
      return Math.floor(Math.random() * 2);
    },
    updateRadioValue(value: string) {
      this.radioValue = value;
    },
    setComputerChoice(value: number) {
      this.computerGuess = this.emojis[value].strength;
      this.computersChoice = this.emojis[value].symbol;
    },
  },
  computed: {
    showingLastMoves(): {
      player: string;
      computer: string;
    }[] {
      if (this.lastMoves.length < 6) {
        return this.lastMoves;
      }
      return this.lastMoves.slice(this.lastMoves.length - 6, this.lastMoves.length - 1);
    },
  },
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.emojis {
  display: flex;
  justify-content: space-between;
  list-style: none;
  padding: 0;
}
.emojis li {
  font-size: 80px;
  transition: transform 0.5s ease-in-out;
}
.emojis li:hover {
  cursor: pointer;
  transform: scale(1.3);
}
.emoji {
  display: flex;
  flex-direction: column;
}
.name {
  font-size: 12px;
  font-weight: bold;
}

.score {
  margin: 150px auto 0 auto;
  border: 3px solid #006d77;
  border-radius: 4px;
  width: 30vh;
  color: #e29578;
  font-size: 46px;
  text-align: center;
  padding: 15px 20px;
  position: relative;
}

.badge {
  background-color: #006d77;
  color: #e29578;
  font-size: 14px;
  padding: 2px 10px;
}

#userLabel {
  position: absolute;
  top: 30px;
  left: -30px;
}

#compLabel {
  position: absolute;
  top: 30px;
  right: -30px;
}

.wrapper {
  min-height: 50px;
  margin: 50px;
  font-size: 60px;
}

.lastMoves {
  border: 3px solid #006d77;
  list-style: none;
  padding: 0;
  font-size: 40px;
  min-height: 310px;
}

span.message {
  display: inline-block;
  min-width: 250px;
}

span.choices {
  display: inline-block;
  min-width: 90px;
}

.radioWrapper {
  text-align: left;
  width: auto;
  margin-top: 3em;
  padding-left: 8em;
}
</style>
