<style scoped>
  .game-container {
    background-image: url('../../assets/images/background_1.jpg');
    background-position: top;
    background-size:cover;
    background-repeat: no-repeat;
    color: white;
  }
  .settings-contain {
    background: #00000075;  
    padding: 5px;
  }
  .top-row {
    justify-content: flex-end;
    background: #00000091;
  }
  .fa-cog {
    font-size: 33px;
    color: #cccccc;
    padding: 8px;
    background: #333333;
    border: 1px solid #565454;
  }
  .fa-cog:hover {
    cursor: pointer;
  }
  .fa-cog.selected {
    background: #313131;
  }
  .main-container {
    display:flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    flex: 0.55;
  }
  .current-question {
    font-size: 100px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  .current-question small {
    font-size: 20px;
  }
  .timer-container {
    position: absolute;
    bottom: 5px;
    left: 0px;
  }
  .users-input {
    background: black;
    border: 1px solid #585858;
    border-radius: 10px;
    width: 70%;
    color: white;
    padding: 15px;
    height: 45px;
    text-align: center;
  }
  .score-container {
    display:flex;
    justify-content: space-between;
    font-size: 35px;
  }
  .correct {
    color:green;
    font-weight: bold;
  }
  .incorrect {
    color:red;
    font-weight: bold;
  }
  .game {
    min-height: 77vh;
    display:flex;
    flex-direction: column;
    align-items: center;
  }
  .counters {
    background: #0000003d;
    padding: 10px;
    border-radius: 1.25rem;
  }
  /*Small devices (landscape phones, 576px and up)*/
  @media (min-width: 576px) {
  }
  /*Medium devices (tablets, 768px and up)*/
  @media (min-width: 768px) {
    .settings-contain {
      padding: 15px;
    }
  }
  /*Large devices (desktops, 992px and up)*/
  @media (min-width: 992px) {
  }
  /*Extra large devices (large desktops, 1200px and up)*/
  @media (min-width: 1200px) { }
</style>
<template>
  <div class="game-container col-12">
    <div class="row top-row">
      <i :class="['fa fa-cog', state == 'settings' ? 'selected' : '']" @click="state = 'settings'"></i>
    </div>
    <settings-container v-show="state == 'settings'" ref="settings" class="settings-contain"></settings-container>
    <div v-show="state == 'playing' && settings" class="mt-2 game">
      <div class="counters col-12 col-md-8">
        <!-- Correct Guesses Bar -->
        <div class="col-9" style="display:inline-block">
          <b-progress :value="game.correct" :max="correctsGoal" variant="success" show-value></b-progress>
        </div>
        <div class="counter col-2 mb-2" style="display:inline-block">Goal:{{correctsGoal}}</div>
        <!-- Streak Bar -->
        <div class="col-9" style="display:inline-block">
          <b-progress :value="game.streak" :max="streakGoal" variant="danger" show-value></b-progress>
        </div>
        <div class="counter col-1 mb-2" style="display:inline-block">x{{game.streakLaps}}</div>
        <!-- Score -->
        <div class="score-container">
          <div class="correct">{{game.correct}}</div>
          <div class="incorrect">{{game.incorrect}}</div>
        </div>
      </div>
      <!-- Main Container -->
      <div class="main-container">
        <!-- Current Question -->
        <div class="current-question">
          {{countdown > 0 ? countdown : game.char.character}}
          <small v-show="game.showAnswer">Answer: "{{game.char ? game.char.romanization : ''}}"</small>
        </div>
        <!-- User Input -->
        <input type="text" class="users-input" v-model="game.userAnswer"/>
      </div>
      <!-- Message -->
      <div v-show="msgs.length" class="msg-container">
        <p v-for="(msg, i) in msgs" class="text-center" :key="'msg-'+i" v-html="msg"></p>
      </div>
      <!-- Timer -->
      <div v-show="timerGoal" class="col-12 timer-container">
        <b-progress :value="game.timer" :max="timerGoal" variant="danger" show-value></b-progress>
      </div>
    </div>
  </div>
</template>

<script>
import Settings from './settings-container';
import { setInterval, clearInterval } from 'timers';
export default {
  components: {
    'settings-container': Settings,
  },
  data() {
    return {
      state: 'settings',
      countdown: 3,
      game: {
        showAnswer: false,
        lastChar:null,
        char:null,
        userAnswer: '',
        streak: 0,
        streakLaps:0,
        correct: 0,
        incorrect: 0,
        chances: 0,
        timer: 0,
      },
      settings: null,
      timerInt: null,
      msgs: [],
      audio: null,
    }
  },
  computed: {
    streakGoal: function() {
      return this.settings ? (this.settings.winConds.streak.set ? parseInt(this.settings.winConds.streak.num) : 10) : 10;
    },
    correctsGoal: function() {
      return this.settings ? (this.settings.winConds.corrects.set ? parseInt(this.settings.winConds.corrects.num) : 25) : 25;
    },
    timerGoal: function() {
      return this.settings ? (this.settings.winConds.timer.set ? parseInt(this.settings.winConds.timer.num) : null) : null;
    }
  },
  mounted() {
    this.$on('play', () => { this.state = 'playing'; });
    //reset msg
    let self = this;
    setInterval(() => {
      if(self.msgs.length) {
        setTimeout(() => {
          self.msgs = [];
        }, 2500);
      }
    }, 5000);
  },
  watch: {
    state: function(newVal, old) {
      if(old == 'settings') {
        //init or re-set settings
        let settings = this.$refs.settings.fetchSettings();
        //first merge all selectedChars together
        let merged = settings.selectedChars.hira.concat(settings.selectedChars.kata);
        merged = merged.concat(settings.selectedChars.kanji);
        settings.selectedChars = merged;

        this.settings = settings;
      }
      if(newVal == 'playing') {
        this.initGame();
      }
    },
    'game.userAnswer': function(newVal) {
      if(newVal == this.game.char.romanization || newVal == this.game.char.character) {
        this.game.userAnswer = '';
        if(this.game.chances == 0) {
          this.game.streak++;
          if(this.game.streak == this.streakGoal) {
            this.game.streakLaps++;
            this.msgs.push('You reached your <strong>STREAK</strong> goal!');
            this.game.streak = 0;
          }
          this.game.correct++;
          if(this.game.correct == this.correctsGoal) {
           this.msgs.push('You reached your <strong>CORRECT GUESSES</strong> goal!');
          }
          this.game.timer = 0;
        }
        this.nextQuestion();
      } else {
        //if the input length is same as answer's, but not the value deem as incorrect
        if(this.game.char.romanization.length <= newVal.length) {
          this.game.chances++;
          this.game.streak = 0;
          this.game.streakLaps = 0;
          //give 3 chances to get correct before showing the answer
          if(this.game.chances > 2 && !this.game.showAnswer) {
            this.game.showAnswer = true;
            this.game.incorrect++;
          }
        }
      }
    }
  },
  methods: {
    initGame() {
      let self = this;
      //reset game values
      this.game = {
        showAnswer: false,
        lastChar:null,
        char:null,
        userAnswer: '',
        streak: 0,
        streakLaps:0,
        correct: 0,
        incorrect: 0,
        chances: 0,
        timer: 0,
      },
      //start countdown
      this.countdown = 3;
      let inv = setInterval(() => {
        self.countdown--;
        if(self.countdown == 0) {
          clearInterval(inv);
          self.nextQuestion();
        }
      },1000);
    },
    nextQuestion() {
      this.game.showAnswer = false;
      this.game.chances = 0;
      if(this.game.char) {
        this.game.lastChar = this.game.char;
      }
      let list = this.settings.selectedChars;
      let chosen = list[Math.floor((Math.random() * list.length))];
      if(this.game.lastChar) {
        if(chosen.id == this.game.lastChar.id) {
          this.nextQuestion();
        } else {
          this.game.char = chosen;
          this.startTimer();
        }
      } else {
        this.game.char = chosen;
        this.startTimer();
      }
      if(this.settings.playSound && this.game.lastChar) {
        this.playSound();
      }
    },
    startTimer() {
      if(this.timerGoal) {
        if(this.timerInt) {
          clearInterval(this.timerInt);
        }
        let self = this;
        this.game.timer = this.timerGoal;
        this.timerInt = setInterval(() => {
          self.game.timer--;
          if(self.game.timer == 0) {
            self.msgs.push('Times up!');
            self.game.showAnswer = true;
            self.game.streak = 0;
            self.game.streakLaps = 0;
            self.game.incorrect++;
            self.game.chances = 3;
            
            clearInterval(self.timerInt);
            self.timerInt = null;
          }
        }, 1000);
      }
    },
    playSound() {
      let self = this;
      if(!this.audio && this.game.lastChar.id.split('-')[0] !== 'ka') {
        this.audio = new Audio('/sounds/'+this.game.lastChar.id.split('-')[0]+'/'+this.game.lastChar.romanization+'.mp3');
        this.audio.play();
        let inv = setInterval(() => {
          if(self.audio) {
            if(self.audio.ended) {
              self.audio = null;
              clearInterval(inv);
            }
          } else {
            clearInterval(inv);
          }
        },10);
      }
    }
  }
}
</script>
