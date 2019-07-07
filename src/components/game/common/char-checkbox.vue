<style scoped>
  .checkbox-contain {
    padding: 5px;
    width:33%;
  }
  .btn {
    background: white;
    width:100%;
    font-size: 100%;
    font-weight: bold;
    border:2px solid #797979;
  }
  .btn:hover {
    cursor: pointer;
  }
  .btn.selected {
    background:green;
    color:white;
  }
  /*Small devices (landscape phones, 576px and up)*/
  @media (min-width: 576px) {
  }
  /*Medium devices (tablets, 768px and up)*/
  @media (min-width: 768px) {
    .checkbox-contain {
      display: flex;
      flex-direction: column;
      justify-content: center;
      width: 20%;
    }
  }
  /*Large devices (desktops, 992px and up)*/
  @media (min-width: 992px) {
    .checkbox-contain {
      width: 14.5%;
    }
  }
  /*Extra large devices (large desktops, 1200px and up)*/
  @media (min-width: 1200px) { }
</style>
<template>
  <div :class="'checkbox-contain'">
    <div :class="['btn text-center ', state ? 'selected' : '']" @click="toggleState()">
      {{char.character}}<br/>
      <small>{{char.romanization}}</small>
      <p v-if="char.meaning"><small>({{char.meaning}})</small></p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    char: {},
    type: {},
    initState: { default: false }
  },
  data() {
    return {
      state: this.initState,
    }
  },
  methods: {
    toggleState() {
      this.state = !this.state;
      this.$parent.$emit('charCheckboxToggled', this.char,  this.type);
    }
  },
}
</script>
