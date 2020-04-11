<template>
  <div class="turntable-canvas-demo">
  	<div class="describe">
  		<p class="b3"># turntable-canvas</p>
			<p class="b2">canvas绘制转盘插件（可配置）</p>
			<p class="b2">Vue中使用方法及配置</p>
			<p class="b1">1.npm下载</p>
				<p class="content">npm i turntable-canvas</p>
			<p class="b1">2.import</p>
				<p class="content">import 'turntable-canvas/css/turntable-canvas.min.css';</p>
				<p class="content">import turntableCanvas from 'turntable-canvas';</p>
			<p class="b2">demo源码地址</p>
				<a href="https://github.com/fwhf/turntable-canvas-demo" target="_blank">demo地址</a>
			<p class="b2">turntable-canvas地址</p>
				<a href="https://github.com/fwhf/turntable-canvas" target="_blank">turntable-canvas</a>
			<p class="b2">个人博客地址</p>
				<a href="https://blog.fwhf.xyz" target="_blank">blog.fwhf.xyz</a>
  	</div>
  	<!--<div class="frame"></div>-->
  	<div class="config">
  		<div class="configButton">
  			<button class="configButtonReset" @click="reset">重置</button><button class="configButtonRun" @click="run">运行</button>
  		</div>
  		<label for="width"><span>转盘width</span><input type="number" id="width" v-model="width"/></label>
  		<label for="height"><span>转盘height</span><input type="number" id="height" v-model="height"/></label>
  		
  		<label for="boundary"><span>防止停止旋转时压住扇形边线，可适当加大</span><input type="number" id="boundary" v-model="boundary"/></label>
  		
  		<label for="zhizhen-width"><span>指针width</span><input type="number" id="zhizhen-width" v-model="zhizhen.width"/></label>
  		<label for="zhizhen-src"><span>指针src</span><input type="text" id="zhizhen-src" v-model="zhizhen.src"/></label>
  		
  		<label for="bg-width"><span>外圈圆边width</span><input type="number" id="bg-width" v-model="bg.width"/></label>
  		<label for="bg-color"><span>外圈圆边color</span><input type="text" id="bg-color" v-model="bg.color"/></label>
  		<label for="bg-lampNum"><span>小灯个数lampNum</span><input type="number" id="bg-lampNum" v-model="bg.lampNum"/></label>
  		<label for="bg-lampColor"><span>小灯颜色lampColor</span><input type="text" id="bg-lampColor" v-model="bg.lampColor"/></label>
  		<label for="bg-lampRadius"><span>小灯半径lampRadius</span><input type="number" id="bg-lampRadius" v-model="bg.lampRadius"/></label>
  		<label for="bg-twinkleTime"><span>小灯闪烁间隔twinkleTime</span><input type="number" id="bg-twinkleTime" v-model="bg.twinkleTime"/></label>
  		
  		<label for="prize-bgColor"><span>扇形bgColor,可多传</span><input type="text" id="prize-bgColor" v-model="prize.bgColor"/></label>
  		<label for="prize-textColor"><span>文字textColor</span><input type="text" id="prize-textColor" v-model="prize.textColor"/></label>
  		<label for="prize-textStyle"><span>文字textStyle</span><input type="text" id="prize-textStyle" v-model="prize.textStyle"/></label>
  		<label for="prize-textTop"><span>文字距离内圈距离textTop</span><input type="number" id="prize-textTop" v-model="prize.textTop"/></label>
  		<label for="prize-imgTop"><span>图片距离内圈距离imgTop</span><input type="number" id="prize-imgTop" v-model="prize.imgTop"/></label>
  		<label for="prize-imgWidth"><span>图片imgWidth</span><input type="number" id="prize-imgWidth" v-model="prize.imgWidth"/></label>
  		<label for="prize-imgHeight"><span>图片imgHeight</span><input type="number" id="prize-imgHeight" v-model="prize.imgHeight"/></label>
  		<div class="prizeList">
  			<div class="prizeListMain">
  				<span class="prizeListMainTitle">奖品列表(必传字段)</span>
  				<div class="prizeListMainOperation">
  					<span @click="del">－</span>
  					<span @click="add">＋</span>
  				</div>
  			</div>
  			<div v-for="(item,index) of prizeList" class="prizeListItem" :key='index'>
  				<span class="prizeListTitle">奖品{{index}}</span>
  				<div class="prizeListContent">
	  				<label :for='prizeListId("text",index)' class="prizeListText">
	  					<span>文字</span>
	  					<input type="text" :id='prizeListId("text",index)' v-model="prizeList[index].text"/>
	  				</label>
	  				<label :for='prizeListId("src",index)' class="prizeListImg">
	  					<span>图片</span>
	  					<input type="text" :id='prizeListId("src",index)' v-model="prizeList[index].imgurl"/>
	  				</label>
  				</div>
  			</div>
  		</div>
  	</div>
  	<!--<div class="frame"></div>-->
  	<div class="effect">
  		<div ref="turntableCanvas"></div>
  	</div>
  </div>
</template>

<script>
import 'turntable-canvas/css/turntable-canvas.min.css';
import turntableCanvas from 'turntable-canvas';
export default {
  data () {
    return {
    	//非必传字段
    	width:300,
    	height:300,
    	boundary:1,
    	zhizhen : {
				width : 50,
				src : 'http://blog.fwhf.xyz/img/turntable-canvas/zhizhen.png'
			},
			bg: {
				width : 20,
				color : 'rgb(255,185,74)',
				lampNum : 12,
				lampColor : ['rgb(255,255,255)','rgb(255,234,119)'],
				lampRadius : 3,
				twinkleTime : 500
			},
			prize: {
				bgColor : ['rgb(255,233,204)','rgb(255,247,235)'],
				textColor : 'rgb(214,155,94)',
				textStyle :  "16px Georgia",
				textTop :  14,
				imgTop : 40,
				imgWidth : 32,
				imgHeight : 32
			},
			//必传字段
			prizeList: [
	      {
					text: '奖品一',
					imgurl: 'http://blog.fwhf.xyz/img/turntable-canvas/koushuidou.png'
	      },
	      {
					text: '奖品二',
					imgurl: 'http://blog.fwhf.xyz/img/turntable-canvas/maozi.png'
	      },
	      {
					text: '奖品一一',
					imgurl: 'http://blog.fwhf.xyz/img/turntable-canvas/naiping.png'
	      },
	      {
					text: '奖品',
					imgurl: 'http://blog.fwhf.xyz/img/turntable-canvas/shuibei.png'
	      },
	      {
					text: '奖',
					imgurl: 'http://blog.fwhf.xyz/img/turntable-canvas/xiaohuangya.png'
	      },
	      {
					text: '奖123',
					imgurl: 'http://blog.fwhf.xyz/img/turntable-canvas/yingerche.png'
	      },
			]
    }
  },
  mounted(){
		this.run();
	},
	methods:{
		prizeListId(value,index){
			return 'prizeList' + value + index
		},
		reset(){
			this.width = 300;
    	this.height = 300;
    	this.boundary = 1;
    	this.zhizhen = {
				width : 50,
				src : 'http://blog.fwhf.xyz/img/turntable-canvas/zhizhen.png'
			};
			this.bg = {
				width : 20,
				color : 'rgb(255,185,74)',
				lampNum : 12,
				lampColor : ['rgb(255,255,255)','rgb(255,234,119)'],
				lampRadius : 3,
				twinkleTime : 500
			};
			this.prize = {
				bgColor : ['rgb(255,233,204)','rgb(255,247,235)'],
				textColor : 'rgb(214,155,94)',
				textStyle :  "16px Georgia",
				textTop :  14,
				imgTop : 40,
				imgWidth : 32,
				imgHeight : 32
			};
			this.prizeList = [
	      {
					text: '奖品一',
					imgurl: 'http://blog.fwhf.xyz/img/turntable-canvas/koushuidou.png'
	      },
	      {
					text: '奖品二',
					imgurl: 'http://blog.fwhf.xyz/img/turntable-canvas/maozi.png'
	      },
	      {
					text: '奖品一一',
					imgurl: 'http://blog.fwhf.xyz/img/turntable-canvas/naiping.png'
	      },
	      {
					text: '奖品',
					imgurl: 'http://blog.fwhf.xyz/img/turntable-canvas/shuibei.png'
	      },
	      {
					text: '奖',
					imgurl: 'http://blog.fwhf.xyz/img/turntable-canvas/xiaohuangya.png'
	      },
	      {
					text: '奖123',
					imgurl: 'http://blog.fwhf.xyz/img/turntable-canvas/yingerche.png'
	      },
	  	]
		},
		run(){
			if(this.bg.lampColor != '' && Object.prototype.toString.call(this.bg.lampColor).replace(/(\[object )|\]/g,"") == 'String'){
				this.bg.lampColor = this.bg.lampColor.split('),');
				for(var i = 0 ; i < this.bg.lampColor.length ; i++){
					if(this.bg.lampColor.length - 1 !== i){
						this.bg.lampColor[i] += ')'
					}
				}
			}
			
			if(this.prize.bgColor != '' && Object.prototype.toString.call(this.prize.bgColor).replace(/(\[object )|\]/g,"") == 'String'){
				this.prize.bgColor = this.prize.bgColor.split('),');
				for(var i = 0 ; i < this.prize.bgColor.length ; i++){
					if(this.prize.bgColor.length - 1 !== i){
						this.prize.bgColor[i] += ')'
					}
				}
			}
			var TurntableCanvasConfig = {
				//canvas宽高（非必传）
				width : this.width,
				height : this.height,
				//防止停止旋转时压住扇形边线，可适当加大（非必传）
				boundary : this.boundary,
				//指针图片的宽及地址（非必传）
				zhizhen : this.zhizhen,
				//转盘背景配置（非必传）
				bg: this.bg,
				//转盘奖品（非必传）
				prize: this.prize,
				//奖品列表(必传)
				prizeList: this.prizeList
			}
			new turntableCanvas(this.$refs.turntableCanvas,TurntableCanvasConfig,(cb)=>{
				//此处放异步请求返回获得了哪个奖项
				console.log('请求中...')
				setTimeout(()=>{
					console.log('请求结束...');
					//假设获得了下标为this.rand(0,this.prizeList.length - 1)的奖，把this.rand(0,this.prizeList.length - 1)传入
					cb(this.rand(0,this.prizeList.length - 1));
				},1000)
			},(res)=>{
				//转盘停止时返回
				console.log('返回值:',res);
			})
		},
		rand(n,m){
			var c = m - n + 1;
			return Math.floor(Math.random() * c + n);
		},
		del(){
			this.prizeList.pop();
		},
		add(){
			this.prizeList.push({
				text: '奖123',
				imgurl: 'http://blog.fwhf.xyz/img/turntable-canvas/yingerche.png'
			})
		}
	}
}
</script>

<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

.turntable-canvas-demo{
	display: flex;
	height:100%;
}

.frame{
	width:10px;
	height:100%;
	background: #e1e4e8;
}

.describe{
	padding-top:30px;
	box-sizing: border-box;
	flex:1;
	height:100%;
	overflow-y: auto;
}
.b3{
	font-size:20px;
	font-weight: bold;
}
.b2{
	font-size:18px;
	font-weight: bold;
}
.b1{
	font-size:16px;
	font-weight: bold;
}
.content{
	font-size: 16px;
}

.config{
	flex:1;
	height:100%;
	overflow-y: auto;
}
.configButton{
	width:90%;
	display: flex;
	justify-content: flex-end;
	padding-top:10px;
	margin:0 auto;
}
.configButton button{
	outline: none;
	width:60px;
	height:30px;
	line-height: 30px;
	margin-left:10px;
	border-radius: 4px;
	cursor: pointer;
}
.configButtonReset{
	border:1px solid #dcdfe6;
	background: #fff;
}
.configButtonRun{
	border:1px solid #3485FB;
	background: #3485FB;
	color:#fff;
}
.config label{
	width:90%;
	display: flex;
	padding-top:10px;
	margin:0 auto;
}
.config label span{
	display: block;
	min-width:100px;
	font-size:14px;
	text-align: left;
	height:30px;
	line-height: 30px;
	padding-right:10px;
}
.config label input{
	flex:1;
	outline: none;
	border:1px solid #dcdfe6;
	border-radius:4px; 
	height:30px;
	line-height: 30px;
	min-width:100px;
	box-sizing: border-box;
	padding:0 10px;
}
.config .prizeList{
	width:90%;
	padding-top:10px;
	margin:0 auto 20px;
}
.config .prizeList .prizeListMain{
	display: flex;
	justify-content: space-between;
	font-size: 14px;
	padding:10px 0;
}
.config .prizeList .prizeListMain .prizeListMainOperation{
	display: flex;
}
.config .prizeList .prizeListMain .prizeListMainOperation span{
	display: block;
	width:18px;
	height:18px;
	border:1px solid #dcdfe6;
	line-height: 18px;
	margin-left:10px;
	cursor: pointer;
}
.config .prizeList .prizeListItem{
	font-size:14px;
	display: flex;
}
.config .prizeList .prizeListTitle{
	width:50px;
	text-align: left;
}
.config .prizeList .prizeListContent{
	padding-top:10px;
	flex:1;
}
.config .prizeList .prizeListItem .prizeListContent label{
	padding:0;
	padding-top:2px;
	display: flex;
	margin:0;
	width:100%;
}
.config .prizeList .prizeListItem .prizeListContent label span{
	width:50px;
	min-width:auto;
}
.config .prizeList .prizeListItem .prizeListContent label input{
	flex:1;
}

.effect{
	flex:1;
	height:100%;
	display: flex;
	justify-content: center;
	align-items: center;
}
</style>
