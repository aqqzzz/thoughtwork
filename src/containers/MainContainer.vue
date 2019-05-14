<template>
  <div class="main">
    <Sidebar :active="active" :show="showSidebar" @change="changeActive" @close="closeSidebar"/>
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
      active: 'AGENT'
    }
  },
  components: {
    Sidebar,
    AgentContainer,
  },
  methods: {
    changeActive (item) {
      this.active = item
    },
    closeSidebar () {
      this.$emit('closeCallback')
    }
  }
}
</script>

<style lang="scss" scoped>
  .main {
    max-width: 1200px;
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

