<template>
  <div>
    <div v-if="direction === 'bottom'" class="cl-alert" :class="{'translate': isTranslate}">
      <div class="title">{{title}}</div>
      <div class="content" :class="{'align-left': contentAlign}">{{content}}</div>
      <div class="btn">
        <div class="cancel" v-if="cancel" @click.stop="onNo">{{cancelText}}</div>
        <div class="confirm" @click.stop="onOk">{{successText}}</div>
      </div>
    </div>
    <div v-if="direction === 'center'" class="cl-alert" :class="{'center': direction === 'center'}">
      <div class="title">{{title}}</div>
      <div class="content" :class="{'align-left': contentAlign}">{{content}}</div>
      <div class="btn">
        <div class="cancel" v-if="cancel" @click.stop="onNo">{{cancelText}}</div>
        <div class="confirm" @click.stop="onOk">{{successText}}</div>
      </div>
    </div>
    <div class="cl-alert-mask" v-if="isOpen" @click="isClose"></div>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        isOpen: false,
        title: '提示',
        content: '',
        cancel: false,
        cancelText: '取消',
        successText: '确定',
        direction: 'bottom',
        maskClose: false,
        onSuccess: null,
        noCancel: null
      }
    },
    computed: {
      contentAlign () {
        if (this.content.length > 20) {
          return true
        }
        return false
      },
      isTranslate () {
        if (this.direction === 'bottom' && this.isOpen) {
          return true
        }
        return false
      }
    },
    methods: {
      show (options) {
        options = options || {}
        console.log(this.direction)
        if (!this.direction) this.direction = 'bottom'
        if (options.title) this.title = options.title
        if (options.cancel) this.cancel = options.cancel
        if (options.cancelText) this.cancelText = options.cancelText
        if (options.successText) this.successText = options.successText
        if (options.direction) this.direction = options.direction
        if (options.onSuccess) this.onSuccess = options.onSuccess
        if (options.noCancel) this.noCancel = options.noCancel
        if (options.maskClose) this.maskClose = options.maskClose
        this.showMask = options.showMask
        this.content = options.content
        this.isOpen = true
      },
      isClose () {
        if (this.maskClose) this.onNo()
      },
      onNo () {
        if (this.noCancel) this.noCancel()
        this.isOpen = false
      },
      onOk () {
        if (this.onSuccess) this.onSuccess()
        if (this.direction === 'center') this.direction = ''
        this.isOpen = false
      }
    }
  }
</script>

<style scoped>
  .cl-alert{
    position: fixed;
    left: 0;
    bottom: 0;
    width: 100%;
    background-color: #fff;
    font-size: 14px;
    z-index: 999;
    -webkit-transition:all .3s ;
    transition:all .3s ;
    -webkit-transform: translateY(100%);
    transform: translateY(100%);
  }
  .cl-alert.translate{
    -webkit-transition:all .3s ;
    transition:all .3s ;
    -webkit-transform: translateY(0);
    transform: translateY(0);
  }
  .cl-alert.center{
    left: 50%;
    max-width: 80%;
    background-color: #fff;
    -webkit-transform: translate(-50%,-150%);
    transform: translate(-50%,-150%);
    -webkit-border-radius: 3px;
    border-radius: 3px;
    overflow: hidden;
  }
  .cl-alert .title{
    text-align: center;
    font-size: 18px;
    font-weight: 400;
    padding: 10px;
    color: #000;
  }
  .cl-alert .content{
    display: -webkit-flex;
    display: flex;
    align-items: center;
    justify-content: center;
    min-height: 50px;
    text-align: center;
    padding: 0 10px;
    margin-bottom: 10px;
    color: #404040;
  }
  .cl-alert .content.align-left{
    text-align: left;
  }
  .cl-alert .btn{
    display: -webkit-flex;
    display: flex;
    height: 40px;
    position: relative;
  }
  .cl-alert .btn::after{
    display: block;
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    border-top: 1px solid rgb(217,217,217);
    content: ' ';
  }
  .cl-alert .btn .cancel,
  .cl-alert .btn .confirm{
    width: 100%;
    text-align: center;
    line-height: 40px;
    font-size: 18px;
    position: relative;
  }
  .cl-alert .btn .confirm{
    color: rgb(23,153,250);
  }
  .cl-alert .btn .cancel::after{
    display: block;
    position: absolute;
    right: 0;
    top: 0;
    height: 100%;
    border-left: 1px solid rgb(217,217,217);
    content: ' ';
  }
  .cl-alert-mask{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,.6);
    z-index: 10;
  }
  @media (-webkit-min-device-pixel-ratio: 1.5), (min-device-pixel-ratio: 1.5) {
    .cl-alert .btn::after{
      -webkit-transform: scaleY(0.7);
      transform: scaleY(0.7);
    }
    .cl-alert .btn .cancel::after{
      -webkit-transform: scaleX(0.7);
      transform: scaleX(0.7);
    }
  }
  @media (-webkit-min-device-pixel-ratio: 2), (min-device-pixel-ratio: 2) {
    .cl-alert .btn::after {
      -webkit-transform: scaleY(0.5);
      transform: scaleY(0.5);
    }
    .cl-alert .btn .cancel::after{
      -webkit-transform: scaleX(0.7);
      transform: scaleX(0.7);
    }
  }
</style>
