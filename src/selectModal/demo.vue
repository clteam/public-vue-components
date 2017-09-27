<template>
  <div class="cl-div">
    选择科目：<input v-model="subjectVal" readonly="readonly" @focus="openSubject" placeholder="请选择科目" type="text">

    <select-modal ref="clselect" title="选择科目" :data="subjectData" :multip="true" @change="getValue">
      <p slot="top" class="tip">*其它城市正在开通中，敬请期待</p>
      <p slot="bottom" class="tip">*其它城市正在开通中，敬请期待</p>
    </select-modal>
  </div>
</template>

<script>
  import selectModal from '@components/selectModal/select-modal'
  export default {
    data () {
      return {
        subjectVal: '',
        subjectData: [
          {
            value: 1,
            label: '科目一'
          },
          {
            value: 2,
            label: '科目二'
          },
          {
            value: 3,
            label: '科目三'
          },
          {
            value: 4,
            label: '科目四'
          }
        ]
      }
    },
    methods: {
      openSubject () {
        this.$refs.clselect.open()
      },
      getValue (obj) {
        console.log(obj)
        this.subjectVal = obj.label
      }
    },
    components: {
      selectModal
    }
  }
</script>

<style scoped>
  @import "../common/public.css";
  .tip{
    text-align: center;
    color: rgb(159,159,159);
  }
</style>