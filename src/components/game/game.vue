<template>
	<div class="game-wrapper">
		<section class="game-page">
			<div class="banner">
				<p class="content clearfix"><span class="count">本关地鼠: <i>{{game.dsnum}}</i></span><span class="score">本关得分：<i>{{score}}</i></span><span class="aim">过关目标分: <i>{{game.goal}}</i></span></p>
			</div>
			<div class="content-wrapper" v-for = "(item,index) in game.questions" v-show = "index == idx">
				<div class="question-sec">
					<dl class="question01">
						<dt>问题{{item.qsnum}}：</dt>
						<dd>{{item.qs}}</dd>
					</dl>
				</div>
				<div class="ds-wrapper clearfix" v-if = "item.answer">
				    <!--对加right，错加wrong-->
					<div class="ds-sec" v-for = "(itm,index) in item.answer" @click="chooseAns(index)">
						<div class="ds-pic" :class="state ? itm.state : ''"><span><i :class="ans[index]"></i></span></div>
						<p class="content">{{itm.ans}}</p>
					</div>
				</div>
			</div>
			<div class="count-wrapper clearfix">
				<div class="time-count">
					<div class="time-pic">
						<i class="pic"></i>
					</div>
					<div class="time-rate"><div class="rate" :style="statusLen"></div><div class="time">{{timeLen}}s</div></div>
				</div>
				<div class="qs-count">
					<div class="qs-rate">{{idx+1}}</div>
					<div class="gk-pic">
						<i class="pic"></i>
					</div>
				</div>
			</div>
		</section>
		<!--弹框 + show-->
		<!--成功 start-->
		<div class="shadow-fixed" :class="{true:'show'}[success]">
			<div class="mask-01"></div>
			<div class="success">
				<div class="title"><i></i></div>
				<p>恭喜你迈出成功的第一步，再接再励！</p>
				<div class="btn"></div>
			</div>
		</div>
		<!--成功 end-->
		<!--失败 start-->
		<div class="shadow-fixed" :class="{true:'show'}[fail]">
			<div class="mask-01"></div>
			<div class="fail">
				<div class="title"><i></i></div>
				<p>挑战失败，多多和你一起，重头再来！</p>
				<div class="btn"></div>
			</div>
		</div>
		<!--失败 end-->
	</div>
</template>

<script>
const ERR_OK = 0

export default {
  props: {},
  data () {
    return {
      game: [],
      idx: 0,
      state: false,
      staLen: 100,
      statusLen: 100,
      timeLen: 30,
      success: false,
      fail: false
    }
  },
  created () {
    let self = this
    setInterval(function () {
      if (self.timeLen > 0) {
        self.timeLen --
        self.staLen = (100 / 30) * (self.timeLen)
        self.statusLen = 'width:' + self.staLen + '%'
      }
    }, 1000)
    this.ans = ['ans-a', 'ans-b', 'ans-c']
    this.$http.get('/api/game').then((response) => {
      response = response.body
      if (response.errno === ERR_OK) {
        this.game = response.data
      }
    })
    console.log(this.game)
  },
  computed: {
    score () {
      let score = 10
      let self = this
// js生命周期问题,此时数据还没渲染过来，self.game.questions为undefined。加计时器让它渲染后遍历。
// 如果有大数据输出，此时应该watch动态观察数据变化。

      setTimeout(function () {
        self.game.questions.forEach((item) => {
          item.answer.forEach((item) => {
            if (item.state) {
              console.log(item.state)
            }
          })
        })
      }, 1000)
      return score
    }
  },
  methods: {
    chooseAns (index) {
      this.state = !this.state
      if (this.state === false) {
        this.idx += 1
      }
      console.log(this.game.questions[this.idx].answer[index])
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.game-wrapper{
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 5.08rem;
	background: url(../../../resource/img/bg.jpg)no-repeat top center;
	background-size: cover;
}
.game-page .banner{
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: .4rem;
	margin: 0 auto;
	background: url(../../../resource/img/banner.png)no-repeat top center;
	background-size: contain;
}
.game-page .banner .content{
	width: 3.2rem;
	padding: .19rem .22rem 0;
	margin: 0 auto;
	color: #4b3519;
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
}
.game-page .banner .content span{
	float: left;
	width: .92rem;
	text-align: center;
}
.game-page .banner .content span.count{
	text-align: left;
}
.game-page .banner .content span.aim{
	text-align: right;
}
.game-page .content-wrapper{
	height: 100%;
	overflow-y: auto;
}
.game-page .question-sec{
	min-height: 1.05rem;
	padding: .2rem .08rem 0;
	-webkit-box-sizing: border-box;
	-moz-box-sizing: border-box;
	box-sizing: border-box;
}
.game-page .question-sec .question01{
	padding: .1rem .13rem .12rem;
	color: #fff;
	font-size: .12rem;
	/*font-weight: bold;*/
	line-height: .18rem;
	background: rgba(0,0,0,.45);
	border-radius: .04rem;
	box-shadow:0px 0px .03rem rgba(79,109,34,.74);
}
.game-page .question-sec .question01 dd{
	line-height: .15rem;
}
.game-page .ds-wrapper{
	padding-top: .22rem;
}
.game-page .ds-wrapper .ds-sec{
	float: left;
	width: 50%;
	-webkit-box-sizing: border-box;
	-moz-box-sizing: border-box;
	box-sizing: border-box;
}
.game-page .ds-wrapper .ds-sec:first-child{
	padding-top: .19rem;
}
.game-page .ds-wrapper .ds-sec:last-child{
	float: left;
	width: 100%;
	padding-top: .14rem;
	text-align: center;
}
.game-page .ds-wrapper .ds-sec .ds-pic{
	padding-left: .12rem;
	text-align: center;
}
.game-page .ds-wrapper .ds-sec:first-child .ds-pic{
	padding-left: .18rem;
}
.game-page .ds-wrapper .ds-sec:last-child .ds-pic{
	padding-left: .15rem;
}
.ds-pic span{
	position: relative;
	display: inline-block;
	vertical-align: top;
	width: .73rem;
	height: .68rem;
	padding-left: .13rem;
	line-height: .94rem;
	text-align: left;
	background: url(../../../resource/img/sd.png)no-repeat center center;
	background-size: contain;
}
.ds-pic span i{
	display: inline-block;
	vertical-align: .02rem;
	width: .46rem;
	height: .43rem;
}
.ds-pic span i.ans-a{
	background: url(../../../resource/img/a.png)no-repeat center center;
	background-size: contain;
}
.ds-pic span i.ans-b{
	background: url(../../../resource/img/b.png)no-repeat center center;
	background-size: contain;
}
.ds-pic span i.ans-c{
	background: url(../../../resource/img/c.png)no-repeat center center;
	background-size: contain;
}
.ds-pic.right span{
	background: url(../../../resource/img/right.png)no-repeat center center;
	background-size: contain;
}
.ds-pic.wrong span{
	background: url(../../../resource/img/wrong.png)no-repeat center center;
	background-size: contain;
}
.ds-pic.right span i,
.ds-pic.wrong span i{
	display: none;
}
.game-page .ds-wrapper .ds-sec .content{
	padding: .03rem;
	font-weight: bold;
	color: #297824;
}
.game-page .ds-wrapper .ds-sec:first-child .content{
	padding: .03rem .18rem 0 .09rem;
}
.game-page .ds-wrapper .ds-sec:nth-child(2) .content{
	padding: .03rem .08rem 0 .18rem;
}
.game-page .count-wrapper{
	position: fixed;
	bottom: 0;
	left: 0;
	width: 100%;
	padding: 0 .07rem .1rem;
	-webkit-box-sizing: border-box;
	-moz-box-sizing: border-box;
	box-sizing: border-box;
}
.game-page .count-wrapper .time-count{
	float: left;
	width: 2.1rem;
	height: .39rem;
	background: url(../../../resource/img/ratebg.png)no-repeat center center;
	background-size: contain;
}
.game-page .count-wrapper .time-count .time-pic{
	float: left;
	padding: .1rem .02rem 0 .12rem;
}
.game-page .count-wrapper .time-rate{
	position: relative;
	float: left;
	width: 1.7rem;
	height: .11rem;
	padding-top: .22rem;
	border-radius: .05rem;
}
.time-rate .rate{
	height: .11rem;
	background: #ce0911;
	border-radius: .05rem;
}
.game-page .count-wrapper .time-rate .time{
	position: absolute;
	bottom: -.03rem;
	color: #f2e551;
	text-shadow: #3a6217 .01rem 0 0,#3a6217 0 .01rem 0,#3a6217 -.01rem 0 0,#3a6217 0 -.01rem 0;
    -webkit-text-shadow: #3a6217 .01rem 0 0,#3a6217 0 .01rem 0,#3a6217 -.01rem 0 0,#3a6217 0 -.01rem 0;
    -moz-text-shadow: #3a6217 .01rem 0 0,#3a6217 0 .01rem 0,#3a6217 -.01rem 0 0,#3a6217 0 -.01rem 0;
}
.game-page .count-wrapper .time-rate .time{
	right: .08rem;
	line-height: .17rem;
}
.game-page .count-wrapper .qs-count{
	position: relative;
	float: right;
	width: .83rem;
	height: .39rem;
	background: url(../../../resource/img/timebg.png)no-repeat center center;
	background-size: contain;
}
.game-page .count-wrapper .qs-count .qs-rate{
	float: left;
	width: .42rem;
	padding: .19rem 0 0 .05rem;
	color: #f2e551;
	text-align: center;
	text-shadow: #3a6217 .01rem 0 0,#3a6217 0 .01rem 0,#3a6217 -.01rem 0 0,#3a6217 0 -.01rem 0;
    -webkit-text-shadow: #3a6217 .01rem 0 0,#3a6217 0 .01rem 0,#3a6217 -.01rem 0 0,#3a6217 0 -.01rem 0;
    -moz-text-shadow: #3a6217 .01rem 0 0,#3a6217 0 .01rem 0,#3a6217 -.01rem 0 0,#3a6217 0 -.01rem 0;
}
.game-page .count-wrapper .time-count .time-pic i.pic{
	display: inline-block;
	width: .21rem;
	height: .21rem;
	background: url(../../../resource/img/clock.png)no-repeat center center;
	background-size: contain;
}
.game-page .count-wrapper .qs-count .gk-pic{
	float: left;
    padding: .09rem 0 0 .03rem;
}
.game-page .count-wrapper .qs-count .gk-pic i.pic{
	display: inline-block;
	width: .2rem;
	height: .22rem;
	background: url(../../../resource/img/question.png)no-repeat center center;
	background-size: contain;
}
.game-page .count-wrapper .time-count .time-pic span,
.game-page .count-wrapper .qs-count .gk-pic span{
	position: relative;
	top: -.06rem;
	font-size: .12rem;
	color: #fff;
	text-shadow: #3a6217 1px 0 0,#3a6217 0 1px 0,#3a6217 -1px 0 0,#3a6217 0 -1px 0;
    -webkit-text-shadow: #3a6217 1px 0 0,#3a6217 0 1px 0,#3a6217 -1px 0 0,#3a6217 0 -1px 0;
    -moz-text-shadow: #3a6217 1px 0 0,#3a6217 0 1px 0,#3a6217 -1px 0 0,#3a6217 0 -1px 0;   
}
.game-page .count-wrapper .time-count .time-pic span i,
.game-page .count-wrapper .qs-count .gk-pic span i{
	display: inline-block;
	-webkit-transform: scale(.8);
    -moz-transform: scale(.8);
    transform: scale(.8);
}
/*弹框 start*/
.shadow-fixed{
	width: 100%;
	height: 100%;
	display: none;
}
.shadow-fixed.show{
	display: block;
}
.shadow-fixed .mask-01{
	position: fixed;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.4) !important;
    filter: Alpha(opacity=40);
    background: #000;
    z-index: 99;
}
.shadow-fixed .success,
.shadow-fixed .fail{
	position: fixed;
	top: 50%;
	left: 19.38%;
	width: 61.25%;
	margin-top: -.79rem;
	padding: 0 .16rem;
	background: #fffce6;
	border-radius: .07rem;
	z-index: 999;
	-webkit-box-sizing: border-box;
	-moz-box-sizing: border-box;
	box-sizing: border-box;
}
.shadow-fixed .success .title,
.shadow-fixed .fail .title{
	width: 1.56rem;
	height: .44rem;
	line-height: .44rem;
	margin: 0 auto;
	margin-top: -.22rem;
	text-align: center;
}
.shadow-fixed .success .title{
	background: url(../../../resource/img/titlebg.png)no-repeat center center;
	background-size: contain;
}
.shadow-fixed .success .title i,
.shadow-fixed .fail .title i{
	display: inline-block;
	vertical-align: middle;
    width: .72rem;
    height: .22rem;
}
.shadow-fixed .success .title i{
	background: url(../../../resource/img/success.png)no-repeat center center;
	background-size: contain;
}
.shadow-fixed .fail .title i{
	background: url(../../../resource/img/lose.png)no-repeat center center;
	background-size: contain;
}
.shadow-fixed .success p,
.shadow-fixed .fail p{
	padding-top: .12rem;
	line-height: .28rem;
	color: #763f27;
	font-size: .16rem;
	text-align: center;
}
.shadow-fixed .fail p{
	padding-top: .07rem;
}
.shadow-fixed .success .btn,
.shadow-fixed .fail .btn{
	width: 1.29rem;
	height: .52rem;
	margin: .06rem auto .1rem;	
}
.shadow-fixed .success .btn{
	background: url(../../../resource/img/btn01.png)no-repeat center center;
	background-size: contain;
}
.shadow-fixed .fail .btn{
	background: url(../../../resource/img/btn02.png)no-repeat center center;
	background-size: contain;
}
/*弹框 end*/
@media only screen and (min-width: 414px) {
	.welc .tip {
	    padding: .1rem .2rem 0;
	}
}
@media only screen and (min-width: 768px) {
	.welc .tip {
	    padding: .1rem .2rem 0;
	}
	.game-page .count-wrapper .qs-count .qs-rate {
	    padding: .2rem 0 0 .05rem;
	}
}
</style>
