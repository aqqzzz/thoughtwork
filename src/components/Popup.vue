<template>
  <div class="wrapper">
    <div :class="{background: visible}"/>
    <div :class="{ 'popup': true, visible }">
      <div class="triangle"/>
      <i class="icon-close" data-index="closePopup" @click="clear"/>
      <div class="content">
        <div>Separate multiple resource names with comma</div>
        <input v-model="input"/>
      </div>
      <div class="footer">
        <div class="confirm btn" data-index="closePopup" @click="addResource">
          Add Resource
        </div>
        <div class="cancel btn" data-index="closePopup" @click="clear">
          Cancel
        </div>
      </div>
    </div>
  </div>
  
</template>
<script>
export default {
  props: {
    visible: Boolean,
    addCallback: Function,
  },
  data() {
    return {
      input: ''
    }
  },
  methods: {
    clear() {
      this.input = ''
    },
    addResource() {
      this.$emit('addCallback', this.input.split(/,|，/))
      this.clear()
    }
  }
}
</script>

<style lang="scss" scoped>
.background {
  display: none;
}
.popup {
  border: 1px solid #00b4cf;
  position: absolute;
  top: 100%;
  width: 300px;
  background-color: white;
  padding: 8px;
  visibility: hidden;
  z-index: 1;
  cursor: default;
  color: #435466;
  font-weight: normal;
  > i {
    float: right;
    color: #00b4cf;
    cursor: pointer;
  }
  .triangle {
    position: absolute;
    top: 0;
    left: 0;
    transform: translate(50%, -100%);
    margin-left: -5px;
    border: 5px solid transparent;
    border-bottom-color: #00b4cf;
  }
  &::before {
    content: '';
    position: absolute;
    top: 1px;
    left: 0;
    transform: translate(50%, -100%);
    margin-left: -5px;
    border: 5px solid transparent;
    border-bottom-color: white;
    z-index: 2;
  }

  
}
.content {
  position: relative;
  width: 100%;
  margin-top: 13px;
  > div {
    margin-bottom: 10px;
  }
  input {
    width: 100%;
    height: 20px;
    color: #00b4cf;
  }
}
.footer {
  margin-top: 10px;
  .btn {
    margin-right: 10px;
    height: 30px;
    line-height: 30px;
    color: white;
    border: none;
    display: inline-block;
    padding: 0 10px;
    cursor: pointer;
  }
  .confirm {
    background-color: #00b4cf;
  }
  .cancel {
    background-color: #2d4050;
  }
}
.visible {
  visibility: visible;
}
@media screen and (max-width: 1024px) {

  .background {
    display: block;
    position: fixed;
    background-color: rgba(0, 0, 0, .8);
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    z-index: 100;
    cursor: default;
  }
  .popup {
    z-index: 101;
    position: fixed;
    left: 50vw;
    top: 50vh;
    transform: translate(-50%, -50%);
    // box-shadow: 0 0 0 50vmax rgba(0,0,0,.8);
    width: 500px;
    .triangle {
      display: none;
    }
    &:before {
      content: none;
    }
  }
}
@media screen and (max-width: 768px) {
  .popup {
    width: 100%;
  }
}
</style>
