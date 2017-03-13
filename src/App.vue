<template>
  <div>
    <div class="wrapper">
    	<div class="guide-wrapper">
				<section class="welc">
					<div class="my-header"></div>
					<div class="welcome"></div>
					<router-link to="/game">
						<div class="btn"></div>
					</router-link>
					<div class="tip">
						<p>找出鼠洞，答案藏在每个鼠洞里，如果答案准确会清除鼠洞里的老鼠，长征路上将少一分障碍。</p>
					</div>
				</section>
			</div>
    </div>
    <keep-alive>
    	<router-view></router-view>
    </keep-alive>
  </div>
</template>

<script>
export default {}
</script>

<style>
@import 'common/stylus/layout.styl';
@import 'common/stylus/activity.styl';

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #fff;
}
@charset "utf-8";
body{
	font-size: .12rem;
	background-color: #61b427;
	font-family: "微软雅黑","Microsoft YaHei", Arial, SimHei;
}
.guide-wrapper{
	width: 100%;
	height: 5.08rem;
	background: url(../resource/img/bg.jpg)no-repeat top center;
	background-size: cover;
}
.game-wrapper{
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 5.08rem;
	background: url(../resource/img/bg.jpg)no-repeat top center;
	background-size: cover;
}
.game-page{
	position: fixed;
	top: 0;
	left: 0;
	bottom: 0;
	width: 100%;
	padding-top: .37rem;
	padding-bottom: .5rem;
}
.welc .my-header{
	width: 3.2rem;
	height: 1.13rem;
	margin: 0 auto;
	background: url(../resource/img/title.png)no-repeat center center;
	background-size: contain;
}
.welc .welcome{
	width: 2.56rem;
	height: 1.41rem;
	margin: .89rem auto .2rem;
	background: url(../resource/img/guide.png)no-repeat center center;
	background-size: contain;
}
.welc .welcome p{
	padding-bottom: .1rem;
}
.welc .btn{
	width: 1.77rem;
	height: .65rem;
	margin: 0 auto;
	background: url(../resource/img/btn.png)no-repeat center center;
	background-size: contain;
}
.welc .tip{
	padding: .1rem .23rem 0;	
}
.welc .tip p{
	padding: .08rem .1rem;
	line-height: .18rem;
	color: #30510f;
	text-align: center;
	background: rgba(93,148,41,.57);
	border-radius: .05rem;
}
</style>
