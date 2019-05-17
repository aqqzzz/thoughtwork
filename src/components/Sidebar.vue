<template>
  <div :class="{container: true, show}">
    <div class="close"><i class="icon-close" @click="closeNav"/></div>
    <div class="navigation">
      <div v-for="item in items" :key="item.text" :class="{ isActive: item.text === active }" @click="choose(item)">
        <i :class="item.icon" />
        <p>{{item.text}}</p>
      </div>
    </div>
    <div class="history">
      <div>History</div>
      <ul>
        <li v-for="(history, index) in histories" :key="index" @click="jumpTo(history)">
          {{history}}
        </li>
      </ul>
    </div>
  </div>
</template>
<script>
export default {
  props: ['active', 'change', 'show', 'close', 'histories'],
  data() {
    return {
      items: [
        {icon: 'icon-dashboard', text: 'DASHBOARD'},
        {icon: 'icon-sitemap', text: 'AGENT'},
        {icon: 'icon-boat', text: 'MY CRUISE'},
        {icon: 'icon-cog', text: 'HELP'}
      ],
    }
  },
  methods: {
    choose (item) {
      this.$emit('change', item.text)
    },
    closeNav () {
      this.$emit('close')
    },
    jumpTo (history) {
      location.href = history
    }
  }
}
</script>

<style lang="scss" scoped>
  .container {
    background-color: #2d4054;
    height: 100%;
    width: 300px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    color: #f3f3f3;
    font-size: 2rem;
  }
  .close {
    display: none;
    height: 40px;
    text-align: right;
    padding: 20px;
    position: absolute;
    width: 100%;
    box-sizing: border-box;
    > i {
      color: #00b4cf;
    }
  }
  .navigation {
    > div {
      width: 100%;
      display: flex;
      
      margin-top: 10px;
      height: 40px;
      align-items: center;
      padding: 0 40px;
      box-sizing: border-box;
      cursor: pointer;
      i {
        margin-right: 10px;
      }
      &:hover {
        background-color: #435466;
        color: #0084cf;
      }
    }
    .isActive {
      background-color: #435466;
      color: #0084cf;
    }
  }
  .history {
    text-align: left;
    font-weight: normal;
    padding-left: 20px;
    margin-bottom: 10px;

    > div {
      margin-bottom: 10px;
    }
    ul {
      margin: 0;
      padding-left: 10px;
      font-size: 1.6rem;
      li {
        cursor: pointer;
        margin-bottom: 1rem;
      }
    }
  }
  @media screen and (max-width: 1024px) {
    .container {
      position: absolute;
      left: 0;
      top: -50px;
      transform: translateX(-100%);
      transition: all 1s ease;
      z-index: 6;
      height: 100vh;
    }
    .close {
      display: block;
    }
    .navigation {
      margin-top: 60px;
    }
    .show {
      transform: translateX(0)
    }
  }
</style>

