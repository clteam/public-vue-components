<template>
  <div>
    <div class="c-date-picker" :class="{'translate': isOpen}">
      <div class="header" v-if="hasHeader">
        <span class="cancel" @click.stop="onCancel">取消</span>
        <span class="title">{{title}}</span>
        <span class="ok" @click.stop="onComplete">完成</span>
      </div>
      <div class="content" ref="contents">
        <slot></slot>
        <date-picker
          :forward="opts.forward"
          :noToday="opts.noToday"
          :backward="opts.backward"
          :format="opts.format"
          :hasHeader="opts.hasHeader"
          :value.sync="opts.date"
          v-model="dateValue"
          @hide="onComplete"></date-picker>
      </div>
    </div>
    <div class="date-picker-mask" :class="{'translate': isOpen}"></div>
  </div>
</template>

<script>
  import datePicker from './date-picker.vue'
  export default {
    props: {
      title: String,
      forward: {
        type: Boolean,
        default: false
      },
      noToday: {
        type: Boolean,
        default: false
      },
      backward: {
        type: Boolean,
        default: false
      },
      hasHeader: {
        type: Boolean,
        default: true
      },
      format: {
        type: String,
        default: 'yyyy-mm-dd'
      },
      value: String
    },
    data () {
      return {
        opts: {
          date: '',
          forward: false,
          noToday: false,
          backward: false,
          format: 'yyyy-mm-dd',
          hasHeader: true
        },
        dateValue: '',
        isOpen: false
      }
    },
    created () {
      if (this.forward) this.opts.forward = this.forward
      if (this.noToday) this.opts.noToday = this.noToday
      if (this.backward) this.opts.backward = this.backward
      if (this.format !== 'yyyy-mm-dd') this.opts.format = this.format
      if (!this.hasHeader) this.opts.hasHeader = this.hasHeader
      if (this.value) this.opts.date = this.value
    },
    methods: {
      open () {
        this.isOpen = true
      },
      hide () {
        this.isOpen = false
      },
      onComplete (value) {
        let dateValue = (toString.call(value) === '[object String]') ? value : this.dateValue
        this.$emit('change', dateValue)
        this.hide()
      },
      onCancel () {
        this.isOpen = false
      }
    },
    components: {
      datePicker
    }
  }
</script>

<style scoped>
  .c-date-picker{
    position: fixed;
    bottom: 0;
    left: 0;
    z-index: 5000;
    background: #fff;
    width: 100%;
    font-size: 16px;
    -webkit-transform: translateY(100%);
    transform: translateY(100%);
    -webkit-transition:all .3s ;
  }
  .c-date-picker.translate{
    -webkit-transform: translateY(0);
    transform: translateY(0);
  }
  .c-date-picker .header{
    height: .90rem;
    line-height: .90rem;
    font-size: 16px;
    border-bottom: 1px solid rgb(217,217,217);
    text-align: center;
  }
  .c-date-picker .header span{
    display: inline-block;
  }
  .c-date-picker .header span.cancel{
    color: rgb(159,159,159);
    width: 20%;
    text-align: left;
  }
  .c-date-picker .header span.ok{
    color: rgb(46,166,242);
    text-align: right;
    width: 20%;
  }
  .c-date-picker .header span.title{
    width: 50%;
    text-align: center;
  }
  .c-date-picker .content{
    display: inline;
    text-align: center;
  }
  .date-picker-mask{
    position: absolute;
    top: 0;
    width: 100%;
    height: 0%;
    left: 0;
    background: rgba(0,0,0,0);
    z-index: 1000;
    -webkit-transition:all .3s ;
    transition:all .3s ;
  }
  .date-picker-mask.translate{
    height: 100%;
    background: rgba(0,0,0,.6);
  }
</style>
