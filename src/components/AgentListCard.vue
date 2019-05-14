<template>
  <div class="container">
    <img class="logo" :src="getLogo">
    <div class="content">
      <div class="info">
        <span class="host">
          <i class="icon-desktop"/>
          {{agent.host}}
        </span>
        <span class="machainType" :style="{ backgroundColor: getWorkType.color}">{{getWorkType.name.toLowerCase()}}</span>
        <span class="ip">
          <i class="icon-info"/>
          {{agent.ip}}
        </span>
        <span class="file">
          <i class="icon-folder"/>
          {{agent.file}}
        </span>
      </div>
      <div class="resources">
        <span :data-index="`addResource ${agent.id}`">
          <i class="icon-plus" :data-index="`addResource ${agent.id}`"/>
          <Popup :visible="isAddResource" @addCallback="addRes"/>
        </span>
        <span v-for="resource in agent.resources" class="resource" :key="resource">
          {{resource}}
          <i class="icon-trash" @click="deleteRes(resource)"/>
        </span>
        <div v-if="showDenyBtn" class="btn">
          <i class="icon-deny"/>
          Deny
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { WORK_TYPE, AGENT_TYPES } from '../containers/AgentContainer';
import Popup from './Popup'
export default {
  props: {
    agent: Object,
    workTypes: Array,
    deleteResource: Function,
    addResource: Function,
    isAddResource: Boolean,
  },
  components: {
    Popup
  },
  computed: {
    getWorkType () {
      return this.workTypes[this.agent.workType]
    },
    getLogo () {
      // return centOS
      return require('../assets/os icons/' + AGENT_TYPES[this.agent.type] + '.png')
    },
    showDenyBtn () {
      return this.agent.workType === WORK_TYPE.BUILDING
    }
  },
  methods: {
    deleteRes(resource) {
      this.$emit('deleteResource', this.agent.id, resource)
    },
    addRes(resource) {
      this.$emit('addResource', this.agent.id, resource)
    }
  }
}
</script>
<style lang="scss" scoped>
.container {
  width: 100%;
  height: 80px;
  position: relative;
  display: flex;
  align-items: center;
  padding: 10px;
  text-align: left;
  box-sizing: border-box;
  font-size: 1.6rem;
}
.logo {
  height: 60px;
  margin-right: 10px;
  flex-shrink: 0;
}
@media screen and (max-width: 1200px) {
  .logo {
    display: none;
  }
}
.content {
  flex-grow: 1;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: flex-start;
  height: 100%;
  .info {
    font-size: 1.8rem;
    > span {
      margin-right: 10px;
    }
    .host {
      color: #00b4cf;
    }
    i {
      color: black;
      margin-right: 5px;
    }
    .machainType {
      color: white;
      font-size: 1.4rem;
      padding: 4px;
    }
  }
  .resources {
    position: relative;
    height: 25px;
    width: 100%;
    text-align: left;
    span:first-child {
      display: inline-block;
      padding: 3px;
      color: white;
      font-weight: bold;
      background-color: #00b4cf;
      vertical-align: middle;
      cursor: pointer;
    }
    .resource {
      display: inline-block;
      height: 100%;
      background-color: #e1e4e6;
      margin-left: 5px;
      vertical-align: middle;
      padding: 0 3px;
      display: inline-flex;
      align-items: center;
      i {
        margin-left: 5px;
        display: inline-block;
        cursor: pointer;
      }
    }
    .btn {
      float: right;
      background-color: #00b4cf;
      color: white;
      padding: 5px;
      border-radius: 5px;
      cursor: pointer;
    }
  }
}

</style>
