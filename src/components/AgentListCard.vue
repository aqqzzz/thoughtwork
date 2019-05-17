<template>
  <div v-if="cardType ==='list'" :class="getContainerClass">
    <img class="logo" :src="getLogo">
    <div class="content">
      <div class="info">
        <span class="host">
          <i class="icon-desktop"/>
          {{agent.host}}
        </span>
        <span class="machainType">
          <span :style="{ backgroundColor: getWorkType.color}">
            {{getWorkType.name.toLowerCase()}}
          </span>
        </span>
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
        
      </div>
      <div v-if="showDenyBtn" class="btn" @click="deny">
        <i class="icon-deny"/>
        Deny
      </div>
    </div>
  </div>
  <div v-else :class="getContainerClass">
    <div class="machainType" :style="{borderTopColor: getWorkType.color, borderLeftColor: getWorkType.color }">
      <span>
        {{getWorkType.name.toLowerCase()}}
      </span>
    </div>
    <div class="btnGroup">
      <span class="btn" :data-index="`addResource ${agent.id}`">
        <i class="icon-plus" :data-index="`addResource ${agent.id}`"/>
        <Popup :visible="isAddResource" @addCallback="addRes" />
      </span>
      <span v-if="showDenyBtn" class="btn" @click="deny">
        <i class="icon-deny"/>
        Deny
      </span>
    </div>
    <img class="logo" :src="getLogo" />
    <div class="info">
      <div class="host">
        <i class="icon-desktop"/>
        {{agent.host}}
      </div>
      <div class="ip">
        <i class="icon-info"/>
        {{agent.ip}}
      </div>
      <div class="file">
        <i class="icon-folder"/>
        {{agent.file}}
      </div>
    </div>
    <div class="resources">
      <span v-for="resource in agent.resources" class="resource" :key="resource">
        {{resource}}
        <i class="icon-trash" @click="deleteRes(resource)"/>
      </span>
      
    </div>
  </div>
</template>
<script>
import { WORK_TYPE, AGENT_TYPES } from '../containers/AgentContainer';
import Popup from './Popup'
import Bus from '../Bus'
export default {
  props: {
    agent: Object,
    workTypes: Array,
    cardType: String, // 'card' | 'list' 当前卡片展示类型
    deleteResource: Function,
    addResource: Function,
    denyBuilding: Function,
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
      return require('../assets/os icons/' + AGENT_TYPES[this.agent.type] + '.png')
    },
    showDenyBtn () {
      return this.agent.workType === WORK_TYPE.BUILDING
    },
    getContainerClass () {
      return `${this.cardType}Container ${this.cardType === 'card' && 'col-md-4 col-sm-12'}`
    }
  },
  methods: {
    deleteRes(resource) {
      this.$emit('deleteResource', this.agent.id, resource)
    },
    addRes(resource) {
      this.$emit('addResource', this.agent.id, resource)
    },
    deny() {
      this.$emit('denyBuiding', this.agent.id)
    }
  }
}
</script>
<style lang="scss" scoped>
.listContainer {
  width: 100%;
  // height: 80px;
  position: relative;
  display: flex;
  align-items: center;
  padding: 10px;
  text-align: left;
  box-sizing: border-box;
  font-size: 1.6rem;

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
        display: inline-block;
        width: 6rem;
        font-size: 1.4rem;
        
        > span {
          padding: 4px;
        }
      }
    }
    .resources {
      position: relative;
      height: 25px;
      width: 100%;
      text-align: left;
      padding-right: 8rem;

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
        height: 100%;
        margin-left: 5px;
      }
      
    }
    .btn {
      position: absolute;
      right: 10px;
      bottom: 10px;
      background-color: #00b4cf;
      color: white;
      padding: 5px;
      border-radius: 5px;
      cursor: pointer;
    }
  }
}
.logo {
  height: 60px;
  margin-right: 10px;
  flex-shrink: 0;
}
.resource {
  display: inline-block;
  background-color: #e1e4e6;
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

.cardContainer {
  padding: 1rem;
  position: relative;
  .machainType {
    border: 30px solid transparent;
    width: 0;
    height: 0;
    position: absolute;
    left: 0;
    top: 0;
    > span {
      background-color: transparent;
      color: white;
      display: inline-block;
      position: absolute;
      left: -3rem;
      top: -2rem;
      transform: rotate(-45deg)
    }
  }
  .btnGroup {
    position: absolute;
    right: 1rem;
    top: 1rem;
    display: flex;
    align-items: center;
    height: 2rem;
    .btn {
      display: inline-block;
      margin-left: 1rem;
      height: 100%;
      padding: 5px;
      background-color: #00b4cf;
      color: white;
      &:first-child {
        width: 2rem;
        text-align: center;
      }
    }
  }
  .info {
    margin-top: 10px;
    > div {
      margin-bottom: 1rem;
    }
    .host {
      color: #00b4cf;
      i {
        color: black;
      }
    }
  }
  .resource {
    margin-right: 5px;
  }
  
}
// @media screen and (min-width: 1024px) {
//   .col-md-4 {
//     width: 30%;
//     margin-right: 3%;
//   }
// }
// @media screen and (max-width: 1024px) {
//   .col-sm-12 {
//     width: 100%;
//   }
// }
@media screen and (max-width: 1200px) {
  .listContainer {
    .logo {
      display: none;
    }
  }
  
}

</style>
