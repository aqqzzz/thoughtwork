<template>
  <div class="main">
    <Sidebar :active="active" :show="showSidebar" :histories="histories" @change="changeActive" @close="closeSidebar"/>
    <AgentContainer v-if="active === 'AGENT'"/>
    <div v-else>
      
    </div>
  </div>
</template>
<script>
import Sidebar from '../components/Sidebar'
import AgentContainer from './AgentContainer'
export default {
  props: {
    showSidebar: Boolean,
    closeCallback: Function,
  },
  data() {
    return {
      active: 'AGENT',
      histories: [location.href]
    }
  },
  components: {
    Sidebar,
    AgentContainer,
  },
  created() {
    if (history){
      history.pushState({}, '', 'AGENT')  
      window.addEventListener('popstate', () => {
        const location = unescape(window.location.pathname)
        this.active = location.slice(1)
      })
    } else {
      location.hash = '#AGENT'
      window.addEventListener('hashchange', () => {
        const location = unescape(window.location.hash)
        this.active = location.slice(1)
      })
    }
    
  },
  methods: {
    changeActive (item) {
      
      if (history) {
        history.pushState({}, '', `/${item}`)
        this.active = item
      } else {
        location.hash = `#${item}`
      }
      if (this.histories.length < 5) {
        this.histories.unshift(location.href)
      } else {
        this.histories.unshift(location.href)
        this.histories.pop()
        console.log(this.histories)
      }
    },
    closeSidebar () {
      this.$emit('closeCallback')
    }
  }
}
</script>

<style lang="scss" scoped>
  .main {
    width: 1200px;
    margin: 0 auto;
    background-color: #f3f3f3;
    flex-grow: 1;
    position: relative;
    display: flex;
    background-color: transparent;
    text-align: left;
  }
  @media screen and (max-width: 1200px) {
    .main {
      width: 100%;
      margin: 0;
    }
    
  }
</style>

