<template>

  <div id="app">
    <div class="left">
      <h1>这里是数据输入:</h1>
      <textarea class="textarea" v-model="UserNameString" placeholder="输入你想要的名字,并以逗号分隔。"></textarea>
      <p>{{UserNameString}}</p>
      <button class="btn" @click="onclick">完成</button>
    </div>
    <div class="right">
    <lottery-turntable id="lottery" :restaraunts="rests" :colors="colors" :key="componentKey"></lottery-turntable>
    </div>
  </div>
</template>

<script>
import LotteryTurntable from './components/lottery/LotteryTurntable'

export default {
  name: 'app',
  data() {
    return {
      componentKey:0,
      UserNameString: '',
      //奖品类别
      rests:[],
      //大转盘奖品区块对应背景颜色
      defaultColors: ["#85C1E9","#FFFAFA","#FAF0E6","#839192","#F8C471","#EE7942","#8B8B00","#FFEC8B","#BCEE68","#27AE60"],
      colors: []
    }
  },
  methods: {
    onclick: function(event){
      this.rests = this.UserNameString.split(',')
      let number = this.rests.length
      this.colors = this.defaultColors
      if (number > 10){ 
        let n = number/10
        for(let i=0;i<n;i++){
          this.colors = this.colors.concat(this.defaultColors)
        }
        let q = number%10
        this.colors = this.colors.concat(this.defaultColors.slice(0,q))
      }
      localStorage.setItem("Userinput",this.UserNameString)
      this.forceRerender()
    },
    forceRerender(){
      this.componentKey += 1
    }
  },
  components: {
    LotteryTurntable
  },
  mounted(){
    if (localStorage.getItem('Userinput')){
        this.UserNameString = localStorage.getItem('Userinput')
        this.rests = this.UserNameString.split(',')
        let number = this.rests.length
        this.colors = this.defaultColors
        if (number > 10){ 
          let n = number/10
          for(let i=0;i<n;i++){
            this.colors = this.colors.concat(this.defaultColors)
          }
          let q = number%10
          this.colors = this.colors.concat(this.defaultColors.slice(0,q))
        }
    }
  },
}
</script>

<style>
  html,body{
    width: 100%;
    height: 100%;
  }
.left {
    float: left;  
    width: 30%;
    height: 100%;
  }
.right {
  float: left;  
  width: 60%;
  height: 100%;
}
#lottery {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.textarea {
  border:0;
  border-radius:5px;
  background-color:rgba(241,241,241,.98);
  width: 70%;
  height: 300px;
  padding: 10px;
  resize: none;
}
.btn {
	width: 160px;
	height: 64px;
	background-image: linear-gradient(135deg, #6555a0 0%, #578aef 55%, #8f41e9 100% );
	border-radius: 8px;
	border:none;
	color:white;
	font-size: 28px;
	box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}
</style>
