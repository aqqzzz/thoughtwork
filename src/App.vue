<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <Header @showNavigation="showNav"/>
    <MainContainer :showSidebar="showNavigation" @closeCallback="closeSidebar"/>
  </div>
</template>

<script>
import Header from './components/Header'
import MainContainer from './containers/MainContainer'
export default {
  name: 'app',
  components: {
    Header,
    MainContainer,
  },
  data() {
    return {
      showNavigation: false
    }
  },
  methods: {
    showNav () {
      this.showNavigation = true
    },
    closeSidebar () {
      this.showNavigation = false
    }
  }
}
</script>

<style lang="scss">
@import './assets/font icons/fonts.css';
html, body {
  height: 100%;
  margin: 0;
  font-size: 10px;
}
div {
  box-sizing: border-box
}
#app {
  font-family: 'cruise', 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background-color: #f3f3f3;
  height: 100%;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  box-sizing: border-box;
  font-size: 1.6rem;
}
@media screen and (max-width: 768px) {
  html, body {
    font-size: 8px;
  }
}
</style>
