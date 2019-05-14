<template>
  <header>
    <div class="title">
      <i class="icon-navicon" @click="showNav"/>
      <img alt="logo" src="../assets/logo/logo.svg" class="logo"/>
      <img alt="avatar" src="../assets/avatar.png" class="user-avatar"/>
    </div>
  </header>
</template>
<script>
export default {
  props: {
    showNavigation: Function,
  },
  methods: {
    showNav () {
      this.$emit('showNavigation')
    }
  }
}
</script>

<style lang="scss" scoped>
  header {
    flex-shrink: 0;
    width: 100%;
    height: 50px;
    top: 0;
    /* border-bottom: 1px solid #e9e9e9; */
    box-shadow: 0px 5px #e9e9e9;
    background-color: white;;
  }
  header .title {
    max-width: 1200px;
    margin: auto;
    height: 100%;
    position: relative;
    display: flex;
    align-items: center;
    justify-content: space-between;
    i {
      display: none;
    }
    &::before {
      content: '';
      height: 100%;
    }

    .logo {
      width: 100px;
      height: 100%;
    }
    .user-avatar {
      width: 40px;
      height: 40px;
      border-radius: 50%;
    }
  }
  @media screen and (max-width: 1024px) {
    header .title {
      i {
        display: inline-block;
      }
      &::before {
        content: none;
      }
    }
  }
</style>


