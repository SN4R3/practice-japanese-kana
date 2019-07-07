<template>
  <div id="app">
    <b-navbar class="navbar" type="dark" variant="dark">
      <b-navbar-brand href="/">
        Japanese Kana Practice Game
      </b-navbar-brand>
      <b-navbar-nav>
      </b-navbar-nav>
    </b-navbar>
    <GamePage/>
  </div>
</template>

<script>
import GamePage from './components/pages/game-page';
export default {
  name: 'app',
  components: {
    GamePage
  }
}
</script>

<style>
  #app {
    background-color:#f8f8f8;
    min-height:100vh;
    padding-bottom:10vh;
  }
  .navbar {
    margin-bottom: 50px;
  }
</style>
