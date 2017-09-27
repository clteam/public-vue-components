<template>
  <div>
    <div class="c-select" v-if="isOpen">
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
    <div class="mask" v-if="isOpen" @click="hide"></div>
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
  .c-select{
    position: fixed;
    bottom: 0;
    left: 0;
    z-index: 5000;
    background: #fff;
    width: 100%;
    font-size: 16px;
  }
  .c-select .header{
    height: .90rem;
    line-height: .90rem;
    font-size: 16px;
    border-bottom: 1px solid rgb(217,217,217);
    text-align: center;
  }
  .c-select .header span{
    display: inline-block;
  }
  .c-select .header span.cancel{
    color: rgb(159,159,159);
    width: 20%;
    text-align: left;
  }
  .c-select .header span.ok{
    color: rgb(46,166,242);
    text-align: right;
    width: 20%;
  }
  .c-select .header span.title{
    width: 50%;
    text-align: center;
  }
  .c-select .content{
    display: inline;
    text-align: center;
  }
  .mask{
    position: fixed;
    z-index: 1000;
    top: 0;
    right: 0;
    left: 0;
    bottom: 0;
    background: rgba(0,0,0,.6);
  }
</style>
