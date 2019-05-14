<template>
  <div class="content">
    <div class="calcModule col-sm-6 col-m-4 col-4" v-for="(work, index) in workTypes" :key="index" :style="{ backgroundColor: work.color }">
      <div>{{work.name}}</div>
      <i :class="work.icon"/>
      <div class="count">{{agents.filter((item) => item.workType === index).length }}</div>
    </div>
    <div class="totalModule">
      <div class="item">
        ALL
        <div>{{agents.length}}</div>
      </div>
      <div class="item">
        PHYSICAL
        <div>{{getPhysicalCount}}</div>
      </div>
      <div class="item">
        VIRTUAL
        <div>{{getVirtualCount}}</div>
      </div>
    </div>
    <div class="searchInput-sm searchInput">
      <i class="icon-search"/>
      <input v-model="searchText"/>
    </div>
    <div class="tab">
      <div class="left">
        <div :class="{ tabItem: true, active: activeTab === -1}" @click="setActiveTab(-1)">All</div>
        <div :class="{ tabItem: true, active: activeTab === 0 }" @click="setActiveTab(0)">Physical</div>
        <div :class="{ tabItem: true, active: activeTab === 1 }" @click="setActiveTab(1)">Virtual</div>
        <div class="searchInput">
          <i class="icon-search"/>
          <input v-model="searchText"/>
        </div>
      </div>
      <div class="right">
        <i class="icon-th-list"/>
        <i class="icon-th-card"/>
      </div>
    </div>
    <div class="agentList" @click="handleClickList">
      <AgentListCard v-for="(agent, index) in getAgentList" :key="index" 
        :agent="agent" 
        :workTypes="workTypes"
        :isAddResource="agent.id === activeAgent"
        @deleteResource="deleteResource"
        @addResource="addResource"
      />
    </div>
  </div>
</template>
<script>
import AgentListCard from '../components/AgentListCard'
const AGENT_TYPE = {
  WINDOWS: 0,
  CENTOS: 1,
  DEBIN: 2,
  SUSE: 3,
  UBUNTU: 4,
}
export const AGENT_TYPES = [
  'windows', 'cent_os', 'debin', 'suse', 'ubuntu'
]
export const WORK_TYPE = {
  BUILDING: 0,
  IDLE: 1,
}
const MACHAIN_TYPE = {
  VIRTUAL: 0,
  PHYSICAL: 1,
}
const WORK_TYPES = [
  {color: '#ff9a2a', icon: 'icon-cog', name: 'Building'},
  {color: '#7fbc39', icon: 'icon-coffee', name: 'Idle'},
]
export default {
  components: {
    AgentListCard
  },
  data() {
    return {
      agents: new Array(8).fill(0).map((_, index) => {
        return {
          id: index,
          type: Math.floor(Math.random() * 4),
          host: 'bjdksjdjdj01.thoughtworks.com',
          workType: Math.floor(Math.random() * 2),
          machainType: Math.floor(Math.random() * 1.9),
          ip: '192.168.1.124',
          file: 'var/lib/cruise-agent',
          resources: ['Firefox', 'Safari', 'Ubuntu', 'Chrome'].slice(0, Math.floor(Math.random() * 4))
        }
      }),
      workTypes: WORK_TYPES,
      activeAgent: -1,
      activeTab: -1,
      searchText: '',
    }
  },
  computed: {
    getPhysicalCount () {
      return this.agents.filter((item) => item.machainType === MACHAIN_TYPE.PHYSICAL).length
    },
    getVirtualCount () {
      return this.agents.filter((item) => item.machainType === MACHAIN_TYPE.VIRTUAL).length
    },
    getCurrentAgents () {
      // 根据判断条件返回对应的agents列表
      return this.agents
    },
    getAgentList () {
      let agents = this.agents
      if (this.activeTab !== -1) {
        agents = agents.filter((item) => item.machainType === this.activeTab)
      }
      if (this.searchText) {
        // 根据搜索条件对对应字段进行搜索
      }
      return agents
    }
  },
  methods: {
    setActiveTab (tab) {
      this.activeTab = tab
    },
    deleteResource (id, resource) {
      const agentIndex = this.agents.findIndex((agent) => agent.id === id)
      const agent = this.agents[agentIndex]
      agent.resources = agent.resources.filter((res) => res !== resource)
      this.agents[agentIndex] = agent
    },
    addResource (id, resources) {
      const agentIndex = this.agents.findIndex((agent) => agent.id === id)
      const agent = this.agents[agentIndex]
      agent.resources = agent.resources.concat(resources)
      // 处理掉其中重复的resource
      agent.resources = agent.resources.filter((item,index) => agent.resources.indexOf(item) === index)
      this.agents[agentIndex] = agent
    },
    handleClickList(e) {
      if (e.target.dataset.index) {
        if (e.target.dataset.index.indexOf('addResource') >= 0) {
          const id = e.target.dataset.index.split(' ')[1]
          this.activeAgent = parseInt(id)
        } else if (e.target.dataset.index.indexOf('closePopup') >= 0) {
          this.activeAgent = -1
        }
      }
      
    },
  }
}
</script>
<style lang="scss" scoped>
.content {
  margin-top: 1rem;
  margin-left: 1rem;
  display:  grid;
  flex-grow: 1;
  grid-template-columns: 1fr 1fr 1fr;
  grid-template-rows: 10rem 4rem auto;
  grid-gap: 2rem;
}
.calcModule {
  position: relative;
  color: white;
  text-align: left;
  font-size: 2rem;
  font-weight: bold;
  padding: 0.5rem 1rem;
  height: 100%;
  box-sizing: border-box;
  align-self: start;
  > div {
    display: inline-block;
    vertical-align: top;
  }
  i {
    font-size: 9rem;
    color: rgba(255, 255, 255, 0.5);
    margin-left: 1rme;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%)
  }
  .count {
    position: absolute;
    bottom: 1rem;
    right: 1rem;
  }
}
.totalModule {
  background: white;
  display: flex;
  padding: 1rem;
  .item {
    flex-grow: 1;
    font-size: 1.4rem;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    padding: 1rem 0;
    > div {
      font-size: 1.6rem;
      font-weight: bold;
    }
  }
}
.searchInput {
  margin-left: 2rem;
  position: relative;
  height: auto;
  padding: 0.5rem;
  font-size: 1.6rem;
  i {
    position: absolute;
    left: 0.7rem;
    top: 0.7rem;
  }
  input {
    padding-left: 3rem;
    background-color: #e9e9e9;;
    width: 20rem;
    height: 2rem;
    border: none;
  }
}
.searchInput-sm {
  display: none;
}
.tab {
  background: white;
  grid-column: 1 / 4;
  display: flex;
  align-items: center;
  justify-content: space-between;
  position: relative;
  font-size: 1.6rem;
  .left {
    height: 100%;
    flex-grow: 1;
    > div {
      display: inline-block;
      height: 100%;
    }
    .tabItem {
      &::before {
        content: '';
        display: inline-block;
        height: 100%;
        vertical-align: middle;
      }
      text-align: center;
      width: 18%;
      max-width: 10rem;
      border-right: 1px solid #e9e9e9;
      cursor: pointer;
    }
    .active {
      border-bottom: 2px solid #00b4cf;
      color: #00b4cf;
    }
    
  }
  .right {
    i {
      margin-right: 1rem;
    }
    .active {
      color: blue;
    }
  }
}
.agentList {
  grid-column: 1 / 4;
  height: auto;
  overflow: auto;
  > div {
    background-color: white;
    margin-bottom: 1rem;
  }
  // background-color: white;
}
@media screen and (max-width: 1200px) {
  .totalModule {
    flex-direction: column;
    .item {
      flex-direction: row;
    }
  }
}
@media screen and (max-width: 1024px) {
  .tab {
    .right {
      display: none;
    }
  }
}
@media screen and (max-width: 768px) {
  .searchInput-sm {
    display: block;
    grid-column: 1 / 3;
    background-color: white;
    margin-left: 0;
    padding: 5px;
    position: relative;
    box-sizing: border-box;
    input {
      width: 100%;
    }
  }
  .content {
    grid-template-columns: 1fr 1fr;
    grid-template-rows: 100px 80px 30px 40px auto;
    grid-gap: 20px;
  }
  .totalModule {
    grid-column: 1 / 3;
    flex-direction: row;
    .item {
      flex-direction: column;
      text-align: center
    }
  }
  .tab {
    grid-column: 1 / 3;
    .left {
      .searchInput {
        display: none
      }
    }
    
  }
  .agentList {
    grid-column: 1 / 3;
  }
}
</style>

