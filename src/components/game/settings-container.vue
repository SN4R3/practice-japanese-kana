<style scoped>
  .charTable {
    display: flex;
    height: 85%;
  }
  .chars-contain {
    display: flex;
    flex-direction: column;
    flex-wrap: wrap;
    max-width: 100vw;
    overflow-x:scroll;
    padding: 0;
    height: 60vh;
    margin-bottom: 25px;
  }
  .chars-contain>* {
    height: 15%;
  }
  .arrow {
    font-size: 44px;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .form-group {
    padding:5px;
  }
  .playbtn {
    display:block;
  }
  .btns-row {
    display:flex;
    justify-content: space-around;
  }
  .btns-row .btn {
    color:white;
    border:1px solid white;
    width: 100%;
    border-radius: 0;
  }
  .btns-row .btn:first-of-type {
    border-right:none;
    border-radius: 0.25rem;
    border-top-right-radius: 0;
    border-bottom-right-radius: 0;
  }
  .btns-row .btn:last-of-type {
    border-left:none;
    border-radius: 0.25rem;
    border-top-left-radius: 0;
    border-bottom-left-radius: 0;
  }
  .btns-row .btn.selected {
    background:white;
    color:black;
  }
  .toggle-col {
    padding:5px;
  }
  .toggle-col:hover {
    cursor: pointer;
  }
  .toggle-col div {
    display: flex;
    flex-direction: column;
    align-items: center;
    background: #ffffff;
    font-size: 75%;
    font-weight: bold;
    border: 2px solid #797979;
    color: #292929;
  }

  /*Small devices (landscape phones, 576px and up)*/
  @media (min-width: 576px) {
  }
  /*Medium devices (tablets, 768px and up)*/
  @media (min-width: 768px) {
    .charTable {
      display: flex;
      height: 57%;
    }
    .chars-contain::-webkit-scrollbar {
      height: 10px;
    }
    /* Track */
    .chars-contain::-webkit-scrollbar-track {
      box-shadow: inset 0 0 5px grey; 
      border-radius: 10px;
    }

    /* Handle */
    .chars-contain::-webkit-scrollbar-thumb {
      background: #218838; 
      border-radius: 10px;
    }
    .form-group input[type=checkbox] {
      transform: scale(0.7);
    }
    .playbtn {
      margin: 0 auto;
      width: 50%;
    }
  }
  /*Large devices (desktops, 992px and up)*/
  @media (min-width: 992px) {

  }
  /*Extra large devices (large desktops, 1200px and up)*/
  @media (min-width: 1200px) { }

  /* Transitions */
  .fade-enter-active, .fade-leave-active {
    transition: opacity .5s;
  }
  .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
    opacity: 0;
  }
</style>
<template>
  <div class="settings-container">
    <div class="btns-row">
      <div :class="['btn', charsShowing == 'hiragana' ? 'selected' : '']" @click="charsShowing = 'hiragana'">Hiragana</div>
      <div :class="['btn', charsShowing == 'katakana' ? 'selected' : '']" @click="charsShowing = 'katakana'">Katakana</div>
      <div :class="['btn', charsShowing == 'kanji' ? 'selected' : '']" @click="charsShowing = 'kanji'">Kanji</div>
    </div>
    <!-- Hiragana -->
    <span v-show="charsShowing == 'hiragana'">
      <h2 class="text-center">Select Hiragana</h2>
      <p class="text-center"><small>{{selectedChars.hira.length}} selected</small></p>
      <div class="charTable">
        <div @click="shiftShowingLeft()" class="arrow arrow-left col-2">
          <i v-show="showLeftArrow" class="fa fa-caret-left"></i>
        </div>
        <div class="chars-contain col-8">
          <template v-for="(char, i) in chars.hiraChars">
            <char-checkbox ref="hiraCheckbox" class="" :key="char.id" :char="char" type="hira"></char-checkbox>
            <div v-if="(i + 1) % 5 === 0" class="toggle-col" :key="'h-col-'+i">
              <div @click="toggleCol('hira',i)">
                <span>Toggle</span><span>Column</span>
              </div>
            </div>
          </template>
        </div>
        <div v-show="showRightArrow" @click="shiftShowingRight()" class="arrow arrow-right col-2">
          <i class="fa fa-caret-right"></i>
        </div>
      </div>
    </span>
    <!-- Katakana -->
    <span v-show="charsShowing == 'katakana'">
      <h2 class="text-center">Select Katakana</h2>
      <p class="text-center"><small>{{selectedChars.kata.length}} selected</small></p>
      <div class="charTable">
        <div @click="shiftShowingLeft()" class="arrow arrow-left col-2">
          <i v-show="showLeftArrow" class="fa fa-caret-left"></i>
        </div>
        <div class="chars-contain col-8">
          <template v-for="(char, i) in chars.kataChars">
            <char-checkbox ref="kataCheckbox" class="" :key="char.id" :char="char" type="kata"></char-checkbox>
            <div v-if="(i + 1) % 5 === 0" class="toggle-col" :key="'k-col-'+i">
              <div @click="toggleCol('kata',i)">
                <span>Toggle</span><span>Column</span>
              </div>
            </div>
          </template>
        </div>
        <div v-show="showRightArrow" @click="shiftShowingRight()" class="arrow arrow-right col-2">
          <i class="fa fa-caret-right"></i>
        </div>
      </div>
    </span>
    <!-- Kanji -->
    <span v-show="charsShowing == 'kanji'">
      <div style="height:20vh;display:flex;justify-content:center;align-items:center;">
        <h2>Coming soon...</h2>
      </div>
      <!-- <h2 class="text-center">Select Kanji</h2>
      <p class="text-center"><small>{{selectedChars.kanji.length}} selected</small></p>
      <div class="charTable">
        <div @click="shiftShowingLeft()" class="arrow arrow-left col-2">
          <i v-show="showLeftArrow" class="fa fa-caret-left"></i>
        </div>
        <div class="chars-contain col-8">
          <template v-for="(char, i) in chars.kanjiChars">
            <char-checkbox ref="kanjiCheckbox" class="" :key="char.id" :char="char" type="kanji"></char-checkbox>
            <div v-if="(i + 1) % 5 === 0" class="toggle-col" :key="'k-col-'+i">
              <div @click="toggleCol('kanji',i)">
                <span>Toggle</span><span>Column</span>
              </div>
            </div>
          </template>
        </div>
        <div v-show="showRightArrow" @click="shiftShowingRight()" class="arrow arrow-right col-2">
          <i class="fa fa-caret-right"></i>
        </div>
      </div> -->
    </span>
    <!-- Goal Settings -->
    <h2 class="text-center">Goal Settings</h2>
    <p class="text-center" style="margin:5px;">Challenge yourself & set your "Win" conditions here</p>
    <!-- Streak -->
    <div class="form-group">
      <label for="streak">Streak:</label>
      <div class="row col-12">
        <transition name="fade">
          <input v-show="winConds.streak.set" type="number" name="streak" class="form-control col-10" v-model="winConds.streak.num"/>
        </transition>
        <input type="checkbox" name="streak" class="form-control col-2 col-md-1" v-model="winConds.streak.set"/>
      </div>
    </div>
    <!-- Correct Guesses -->
    <div class="form-group">
      <label for="corrects">Correct Guesses:</label>
      <div class="row col-12">
        <transition name="fade">
          <input v-show="winConds.corrects.set" type="number" name="corrects" class="form-control col-10" v-model="winConds.corrects.num"/>
        </transition>
        <input type="checkbox" name="corrects" class="form-control col-2 col-md-1" v-model="winConds.corrects.set"/>
      </div>
    </div>
    <h2 class="text-center">Other Settings</h2>
    <!-- Timer -->
    <div class="form-group">
      <label for="timer">Timer per Character: <small>(in seconds)</small></label>
      <div class="row col-12">
        <transition name="fade">
          <input v-show="winConds.timer.set" type="number" name="timer" class="form-control col-10" v-model="winConds.timer.num"/>
        </transition>
        <input type="checkbox" name="timer" class="form-control col-2 col-md-1" v-model="winConds.timer.set"/>
      </div>
    </div>
    <!-- Sound Setting -->
    <div class="form-group">
      <label for="sound">Play Sound:</label>
      <input type="checkbox" name="sound" class="form-control col-2 col-md-1" v-model="playSound"/>
    </div>
    <!-- Errors -->
    <div v-show="errMsg.length" class="alert alert-danger">
      {{errMsg}}
    </div>
    <!-- Play Button -->
    <div class="btn btn-success playbtn" @click="play()">Play Now</div>
  </div>
</template>

<script>
import CharCheck from './common/char-checkbox';
import axios from 'axios'

export default {
  components: {
    'char-checkbox': CharCheck
  },
  data() {
    return {
      charsShowing: 'hiragana',
      chars: {
        hiraChars: [],
        kataChars: [],
        kanjiChars: [],
      },
      selectedChars: {
        hira:[],
        kata:[],
        kanji:[],
      },
      winConds: {
        streak: {
          set: false,
          num: 0,
        },
        corrects: {
          set: false,
          num: 0
        },
        timer: {
          set: false,
          num: 0
        },
      },
      playSound:true,
      errMsg: '',
    }
  },
  computed: {
    showLeftArrow: function() {
      return true;
    },
    showRightArrow: function() {
      return true;
    }
  },
  mounted() {
    axios.get('./chars.json').then(resp => {
      this.chars.hiraChars = resp.data[0].hiragana;
      this.chars.kataChars = resp.data[1].katakana;
      this.chars.kanjiChars = resp.data[2].kanji;
    });
    //listners
    this.$on('charCheckboxToggled', (char, type) => {
      if(type == 'hira') {
        this.selectedChars.hira = this.toggleCharIntOut(this.selectedChars.hira, char);
      } else if(type == 'kata') {
        this.selectedChars.kata = this.toggleCharIntOut(this.selectedChars.kata, char);
      } else if(type == 'kanji') {
        this.selectedChars.kanji = this.toggleCharIntOut(this.selectedChars.kanji, char);
      }
    });
  },
  methods: {
    toggleCol(type, col) {
      //find the checkboxes from col to col - 4 & toggle them
      let items = [];
      if(type == 'hira') {
        items = this.$refs.hiraCheckbox;
      } else if(type == 'kata') {
        items = this.$refs.kataCheckbox;
      } else if(type == 'kanji') {
        items = this.$refs.kanjiCheckbox;
      }
      items.forEach((item, i) => {
        if(i <= col && (col - 4) <= i) {
          item.toggleState();
        }
      });
    },
    toggleCharIntOut(list, item) {
      let exists = false;
      list.forEach((listItem, i) => {
        if(item.id == listItem.id) {
          //take out the char from list
          exists = true;
          list.splice(i, 1);
        }
      });
      if(!exists) {
        list.push(item);
      }
      return list;
    },
    //method that returns all settings to the container
    fetchSettings() {
      return {
        selectedChars:this.selectedChars,
        winConds:this.winConds,
        playSound:this.playSound,
      }
    },
    shiftShowingLeft(type) {
      return type;
    },
    shiftShowingRight(type) {
      return type;
    },
    play() {
      this.errMsg = '';
      //check that characters have been selected
      if(this.selectedChars.hira.length || this.selectedChars.kata.length || this.selectedChars.kanji.length) {
        this.$parent.$emit('play');
      } else {
        this.errMsg = 'You must first select characters you want to study!';
      }
    }
  }
}
</script>
