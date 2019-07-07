<style scoped>
  .page-container {
    display:flex;
    padding:0;
  }
  /*Small devices (landscape phones, 576px and up)*/
  @media (min-width: 576px) {
  }
  /*Medium devices (tablets, 768px and up)*/
  @media (min-width: 768px) {
  }
  /*Large devices (desktops, 992px and up)*/
  @media (min-width: 992px) {
    .page-container {
      justify-content: center;
      align-items:center;
    }
  }
  /*Extra large devices (large desktops, 1200px and up)*/
  @media (min-width: 1200px) { }
</style>
<template>
  <div class="page-container container">
    <game-container></game-container>
  </div>
</template>

<script>
import GameContainer from '../game/game-container';
export default {
  components: {
    'game-container': GameContainer
  },
  props: {
    showFirst: {}
  },
  data() {
    return {

    }
  },
  computed: {},
  created() {},
  mounted() {},
  methods: {}
}
</script>
