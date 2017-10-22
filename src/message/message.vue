<template>
    <div>
      <span v-if="isShow" class="cl-message" :style="styleObj" :class="{show: isShow}">{{content}}</span>
    </div>
</template>

<script>
  export default {
    data() {
      return {
        isShow: false,
        content: '',
        duration: 3,
        styleObj: {
          backgroundColor:  'rgba(138, 138, 139, 1)'
        }
      }
    },
    methods: {
      show (config) {
        if (!this._isEmptyObj(config)) {
          if (typeof config === 'string') {
            this.content = config
            this.duration = 3
          } else {
            this.content = config.content
            if (config.duration) this.duration = config.duration
            if (config.themes === 'white') {
              this.styleObj.backgroundColor = '#fff'
              this.styleObj.color = '#0d2e44'
            }
            if (config.bgColor) this.styleObj.backgroundColor = config.bgColor
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
    transform: translate(-50%,-100%);
    position: absolute;
    top: -100%;
    left: 50%;
    color: #fff;
    padding: 5px 10px;
    font-size: 12px;
    border-radius: 5px;
    -webkit-border-radius: 5px;
    z-index: 100;
    box-shadow: 1px 1px 5px rgba(138, 138, 139, 1);
  }
  .show{
    transform: translate(-50%,0%);
    top: 20%;
  }

</style>