<template>
  <div class="cl-answer">
    <h2>
      <div class="nav-page" v-if="currentIndex"><em>{{dataIdnex + 1}}</em>/{{total}}</div> {{data.question}}
    </h2>
    <div class="question-img" v-show="data.media">
      <img :src="data.media" style="" alt="">
    </div>
    <div class="option" ref="option">
      <ul :class="{'zebra-li': zebra}">
        <li ref="A" @click="selectedItem('A',$event)">
            <span class="item">
              <img v-show="item.A === 'error'" :src="errorIcon" style="width: 25px;"/>
              <img v-show="item.A === 'success'" :src="successIcon" style="width: 25px;"/>
              <em v-show="!item.A">A</em>
            </span>
          <p>{{data.options['A']}}</p>
        </li>
        <li ref="B" @click="selectedItem('B',$event)">
            <span class="item">
              <img v-show="item.B === 'error'" :src="errorIcon" style="width: 25px;"/>
              <img v-show="item.B === 'success'" :src="successIcon" style="width: 25px;"/>
              <em v-show="!item.B">B</em>
            </span>
          <p>{{data.options['B']}}</p>
        </li>
        <template v-if="data.type !== '0'">
          <li ref="C" @click="selectedItem('C',$event)">
            <span class="item">
              <img v-show="item.C === 'error'" :src="errorIcon" style="width: 25px;"/>
              <img v-show="item.C === 'success'" :src="successIcon" style="width: 25px;"/>
              <em v-show="!item.C">C</em>
            </span>
            <p>{{data.options['C']}}</p>
          </li>
          <li ref="D" @click="selectedItem('D',$event)">
            <span class="item">
              <img v-show="item.D === 'error'" :src="errorIcon" style="width: 25px;"/>
              <img v-show="item.D === 'success'" :src="successIcon" style="width: 25px;"/>
              <em v-show="!item.D">D</em>
            </span>
            <p>{{data.options['D']}}</p>
          </li>
        </template>
      </ul>
    </div>

    <hr class="split"/>

    <div class="answer">
      <div class="answer-desc" v-show="isShowAnswer">
        <div class="head">
          <h3>官方解析<i></i></h3>
          <div class="star">
            难度
            <template v-for="(index, item) in parseInt(data.difficulty)">
              <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABoAAAAZCAMAAAAYAM5SAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyhpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMTMyIDc5LjE1OTI4NCwgMjAxNi8wNC8xOS0xMzoxMzo0MCAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTUuNSBNYWNpbnRvc2giIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6MzQwNjUxOTA4QURDMTFFN0E2MzVBRjRFREYzMkJDOUQiIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6MzQwNjUxOTE4QURDMTFFN0E2MzVBRjRFREYzMkJDOUQiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0ieG1wLmlpZDozNDA2NTE4RThBREMxMUU3QTYzNUFGNEVERjMyQkM5RCIgc3RSZWY6ZG9jdW1lbnRJRD0ieG1wLmRpZDozNDA2NTE4RjhBREMxMUU3QTYzNUFGNEVERjMyQkM5RCIvPiA8L3JkZjpEZXNjcmlwdGlvbj4gPC9yZGY6UkRGPiA8L3g6eG1wbWV0YT4gPD94cGFja2V0IGVuZD0iciI/PshJzN4AAACcUExURf////yhKP/+/f7etv7hvf27afynOPynOf3Tnf7mx//p0f2wTfyjLfylM/7kxP7Zqv/9+//u2/2sRf7et/2tRv/v3f7Plf7z5P/58v7Mjv26ZvyiK/ykMP28bP7csv2qQf/8+fy0Wf3Vo/yjL/yzV/3Vov/p0P3Uof///v3Xp/yzVv2wUP3HhP716f716v2wT/7nyP3Hg/7SnP3YqEAg/gEAAADCSURBVHjadNLXEoIwEAXQXYr03lFUwN7L//+bgIxuINyXJHsmmZ0kACQPDSYibE7CBImIIl/WIWK45lKFTSqepEpLSsqhDLtkY4kWX1pEtJpIcbHCX1ZFLCVtfZ/PkBMl34LHFcSZB7CzeGLp7ZF2PZba7i9oPpT578KcITn/9l+sqPQ5TComfZoru+tGSGdJJxSwFBBSp9u4dBX/+Pa7yZOQ3KzvSxfAXZ6bqUzIwFLqOxakEg26i/mAmnxoh48AAwAAvwjhtgYzqQAAAABJRU5ErkJggg==" alt="">
            </template><template v-for="(index, item) in (5 - data.difficulty)">
            <img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABoAAAAZCAMAAAAYAM5SAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyhpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMTMyIDc5LjE1OTI4NCwgMjAxNi8wNC8xOS0xMzoxMzo0MCAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTUuNSBNYWNpbnRvc2giIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6MzQwNjUxOEM4QURDMTFFN0E2MzVBRjRFREYzMkJDOUQiIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6MzQwNjUxOEQ4QURDMTFFN0E2MzVBRjRFREYzMkJDOUQiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0ieG1wLmlpZDozNDA2NTE4QThBREMxMUU3QTYzNUFGNEVERjMyQkM5RCIgc3RSZWY6ZG9jdW1lbnRJRD0ieG1wLmRpZDozNDA2NTE4QjhBREMxMUU3QTYzNUFGNEVERjMyQkM5RCIvPiA8L3JkZjpEZXNjcmlwdGlvbj4gPC9yZGY6UkRGPiA8L3g6eG1wbWV0YT4gPD94cGFja2V0IGVuZD0iciI/Pv92jD4AAACiUExURf///7CwsNDQ0LKysv39/ePj4/7+/tnZ2bW1tejo6O3t7eXl5ff398PDw8bGxsXFxdvb27q6ur29vd7e3ry8vOnp6dzc3Lm5ucLCwsTExMDAwNbW1sfHx7+/v9HR0fr6+snJyebm5vX19be3t9XV1c/Pz87OzuTk5PHx8czMzO7u7uHh4eDg4Li4uPLy8uLi4rGxsdTU1LOzs8jIyOrq6t3d3eWneZ8AAAD9SURBVHjadJHlmoMwFETvQYqVAnV3d9v3f7WFsl8TuvT+SDI5kclERKvnU75UZTSqfEEXuJQTK4HEKkUxUURcRiYusxnupATdeIg8uP0nZ5O9yB7zrM/Og8FwAetsvIbFcBDMX5dHLlnVjXydUX9JN4pl65JMHa+qDql6zjTB3Yr4d1afF6+4+1nvmRhFYmB6+ci+YuvE5vrWDWgp4kFDKYdQiRBH/w6Td64Wpv41VU5KnNBeIj5dJbr4BU+7tN2Mx5u02xX8Oizl2MsC6h1lWbBh0A6h89PoQNguJFBL1zcPqUnrkKVb01CTfvDnuBL0aeq7WnpQrXzXrwADALrLDQnB8CEjAAAAAElFTkSuQmCC" alt="">
          </template>
          </div>
        </div>
        <p class="desc">{{data.comments}}</p>
      </div>
    </div>
  </div>
</template>

<script>
  const _storage = sessionStorage
  export default {
    props: {
      data: Object,
      total: {
        type: Number,
        default: 0
      },
      zebra: { // 斑马线
        type: Boolean,
        default: true
      },
      currentIndex: { // 当前页码
        type: Boolean,
        default: true
      }
    },
    data () {
      return {
        dataIdnex: 0,
        okAnswer: this.data.answer,
        isShowAnswer: false,
        isAnswer: false,
        item: {A: '', B: '', C: '', D: ''},
        successIcon: 'data:image/png;base64,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',
        errorIcon: 'data:image/png;base64,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'
      }
    },
    mounted () {
      this._isExecAnswer()
    },
    methods: {
      selectedItem (answer, event) {
        if (this.isShowAnswer) return
        this._operaDom(answer, event)
        let saveDate = _storage.getItem('exec_answer')
        if (saveDate) saveDate = JSON.parse(saveDate)
        if (!saveDate || saveDate.length === 0) saveDate = []
        for (let i = 0; i < saveDate.length; i++) {
          if (saveDate[i].index === this.dataIdnex) return
        }
        let obj = {
          'index': this.dataIdnex,
          'okAnswer': this.okAnswer,
          'answer': answer
        }
        saveDate.push(obj)
        _storage.setItem('exec_answer', JSON.stringify(saveDate))
        this.$emit('change', answer, this.okAnswer)
      },
      resetData () {
        this.isAnswer = this.isShowAnswer = false
        this.item = {A: '', B: '', C: '', D: ''}
        let items = this.$refs.option.querySelectorAll('li')
        for (let i = 0; i < items.length; i++) {
          items[i].className = ''
        }
      },
      _operaDom (answer, event) {
        this.isAnswer = this.isShowAnswer = true
        if (answer === this.okAnswer) {
          this.$refs[this.okAnswer].className = 'active-success'
          this.item[answer] = 'success'
        } else {
          this.item[answer] = 'error'
          this.$refs[answer].className = 'active-error'
          this.$refs[this.okAnswer].className = 'active-success'
          this.item[this.okAnswer] = 'success'
        }
      },
      _isExecAnswer () {
        let isExecAnswer = _storage.getItem('exec_answer')
        if (isExecAnswer) isExecAnswer = JSON.parse(isExecAnswer)
        if (!isExecAnswer || isExecAnswer.length === 0) return
        for (let i = 0; i < isExecAnswer.length; i++) {
          if (isExecAnswer[i].index === this.dataIdnex) {
            this.$refs[isExecAnswer[i].answer].click()
            this.isAnswer = true
            return
          }
        }
      }
    }
  }
</script>

<style scoped>
  .cl-answer{
    width: 100%;
    height: 100%;
    max-width: 640px;
    background: #f5f6f7;
    position: relative;
  }
  .cl-answer h2{
    font-size: 16px;
    padding: 10px .21rem 0 .21rem;
    line-height: .53rem;
    font-weight: bold;
    background-color: #fff;
    /*margin-top: 10px;*/
    /*margin-bottom: 10px;*/
  }
  h2 .nav-page{
    display: inline-block;
    border: 1px solid #ccc;
    line-height: 16px;
    padding: 3px 10px 1px 10px;
    -webkit-border-radius: 15px;
    -moz-border-radius: 15px;
    border-radius: 15px;
  }
  h2 .nav-page em{
    color: #38a8ef;
  }

  .question-img{
    padding: 0 .90rem;
  }
  .question-img img{
    width: 100%;
  }

  .option{
    padding: .2rem 0 .2rem 0;
    background-color: #fff;
  }
  .option li{
    line-height: .90rem;
    font-size: 16px;
    clear: both;
  }
  .option .zebra-li li:nth-child(odd){
    background: #f7f7f7;
  }

  .option li .item{
    width: 22px;
    line-height: 21px;
    font-size: 14px;
    vertical-align: middle;
    margin: .21rem .21rem 0 .35rem;
    text-align: center;
    border-radius: 50%;
    float: left;
    /*box-shadow: 0 1px 1px 1px #ccc;*/
  }
  .option li p{
    padding-left: 1.17rem;
    line-height: 22px;
    padding: .21rem .21rem;
  }
  .option li .item img,
  .option li .item em{
    display: inline-block;
    vertical-align: baseline;
  }
  .option li .item em{
    display: inline-block;
    width: 100%;
    border: 1px solid #ccc;
    border-radius: 50%;
  }

  .option li.active-success{
    color: #09c800;
  }
  .option li.active-success .ck{
    background: #09c800;
    color: #fff;
  }
  .option li.active-error{
    color: #ff2925;
  }
  .option li.active-error .ck{
    background: #ff2925;
    color: #fff;
  }

  .answer{
    background: #f5f6f7;
  }
  .answer-desc{
    padding: .29rem .24rem;
    background-color: #fff;
  }
  .answer-desc .head{
    display: flex;
    justify-content: space-between;
    height: 14px;
    line-height: 14px;
    margin-bottom:23px;
  }
  .answer-desc .head .star img{
    display: inline-block;
    width: 14px;
    vertical-align: inherit;
  }
  .answer-desc h3{
    padding-left: 9px;
    font-size: 16px;
    line-height: 16px;
    position: relative;
  }
  .answer-desc h3 i{
    position: absolute;
    display: inline-block;
    width: 4px;
    height: 14px;
    background: #38a8ef;
    left: 0;
    top: 0;
  }
  .answer-desc .star{
    font-size: 16px;
    color: #a5a5a5;
  }
  .answer-desc .desc{
    font-size: 16px;
    line-height: 25px;
    padding: 0 .16rem;
  }
  .answer-desc .more-btn{
    display: block;
    height: .86rem;
    line-height: .86rem;
    background-color: #41aaec;
    font-size: 18px;
    color: #fff;
    text-align: center;
    border-radius: 4px;
    width: 87%;
    margin: .42rem auto .46rem auto;
  }
  .answer-desc .more-btn:visited,
  .answer-desc .more-btn:hover,
  .answer-desc .more-btn:active{
    color: #fff;
  }
  hr.split{
    height: .23rem;
    background: #f5f6f7;
    border: none;
    margin: 0;
  }
</style>
