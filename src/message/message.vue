<template>
    <div>
      <span v-if="isShow" class="cl-message default"
            :class="{'white': themes === 'white'}">{{content}}</span>
    </div>
</template>

<script>
  export default {
    data() {
      return {
        isShow: false,
        content: '下水道是',
        duration: 3,
        themes: 'default'
      }
    },
    methods: {
      show (config) {
        if (!this._isEmptyObj(config)) {
          if (typeof config === 'string') {
            this.content = config
          } else {
            this.content = config.content
            if (config.duration) this.duration = config.duration
            if (config.themes === 'white') this.themes = config.themes
          }
        }
        this.isShow = true
        let timerId = setTimeout(() => {
          this.isShow = false
          clearTimeout(timerId)
          if (config.onClose) {
            config.onClose()
          }
        }, this.duration * 1000)
      },
      _isEmptyObj (obj) {
        if (!obj) return true
        return Object.keys(obj).length > 0 ? false : true
      }
    }
  }
</script>

<style scoped>
  .cl-message{
    position: fixed;
    top:30%;
    left: 50%;
    -webkit-transform: translate(-50%, -50%);
    transform: translate(-50%, -50%);
    color: #fff;
    background-color: rgba(0, 0, 0, .7);
    padding: 5px 10px;
    font-size: 12px;
    border-radius: 3px;
    -webkit-border-radius: 3px;
    z-index: 100;
    box-shadow: 0px 0px 10px rgba(138, 138, 139, 1);
  }
  .cl-message.white{
    color: #000;
    background-color: rgba(255, 255, 255, .7);
  }
</style>
