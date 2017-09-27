<template>
  <div class="cl-div">
    选择时间：<input v-model="dateVal" readonly="readonly" @focus="openDatePicker" placeholder="请选择" type="text">

    <cl-date-picker ref="datepicker"
                    title="至少提前10天预约"
                    :forward="true"
                    :hasHeader="true"
                    @change="changeDatePicker">
      <p class="date-picker-desc">国庆期间不能预约ßß</p>
    </cl-date-picker>
  </div>
</template>

<script>
  import clDatePicker from '@components/datePicker/cl-date-picker'
  export default {
    data () {
      return {
        dateVal: ''
      }
    },
    methods: {
      openDatePicker () {
        this.$refs.datepicker.open()
      },
      changeDatePicker (value) {
        console.log(value)
        this.dateVal = value
      }
    },
    components: {
      clDatePicker
    }
  }
</script>

<style scoped>
  @import "../../assets/css/common.css";
  .date-picker-desc{
    padding-left: .37rem;
    padding-top: .20rem;
    line-height: 14px;
    text-align: center;
    color: rgb(159,159,159);
  }
</style>
