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
              <img v-show="item.A === 'error'" src="./error.png" style="width: 25px;"/>
              <img v-show="item.A === 'success'" src="./success.png" style="width: 25px;"/>
              <em v-show="!item.A">A</em>
            </span>
          <p>{{data.options['A']}}</p>
        </li>
        <li ref="B" @click="selectedItem('B',$event)">
            <span class="item">
              <img v-show="item.B === 'error'" src="./error.png" style="width: 25px;"/>
              <img v-show="item.B === 'success'" src="./success.png" style="width: 25px;"/>
              <em v-show="!item.B">B</em>
            </span>
          <p>{{data.options['B']}}</p>
        </li>
        <template v-if="data.type !== '0'">
          <li ref="C" @click="selectedItem('C',$event)">
            <span class="item">
              <img v-show="item.C === 'error'" src="./error.png" style="width: 25px;"/>
              <img v-show="item.C === 'success'" src="./success.png" style="width: 25px;"/>
              <em v-show="!item.C">C</em>
            </span>
            <p>{{data.options['C']}}</p>
          </li>
          <li ref="D" @click="selectedItem('D',$event)">
            <span class="item">
              <img v-show="item.D === 'error'" src="./error.png" style="width: 25px;"/>
              <img v-show="item.D === 'success'" src="./success.png" style="width: 25px;"/>
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
              <img src="./star_1.png" alt="">
            </template><template v-for="(index, item) in (5 - data.difficulty)">
            <img src="./star_0.png" alt="">
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
        item: {A: '', B: '', C: '', D: ''}
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
