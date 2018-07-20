<template>
  <div class="hello" ref='sroll'>
 		<h3>糗事百科</h3>
 		<div v-for="val,index  in arr" @click="btn(index)">
 		<p>{{val.content}}</p>
 		<p>{{val.dateandtime}}</p>
 		</div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data () {
    return {
      arr:[],
      num:1,
      reapt: 1
    }
  },
  created(){
  	var that = this
  	console.log(that.num)
  	axios.get('http://guoxiao158.top/joke/getjoke.php').then((res)=>{
  		this.arr=res.data.dataList
  		console.log(res)
  	})
  },
  methods:{
  	btn(i){
  		console.log(i)
  		this.$router.push({path:"/tab",query:{id:i}})
  	}
  },
  mounted(){
  	this.$refs.sroll.addEventListener('scroll', () =>{
  		
  		let a = this.$refs.sroll.scrollHeight
  		let b = document.documentElement.clientHeight
  		let c = this.$refs.sroll.scrollTop

	  	if(a-(b+c) < 50 && this.reapt == 1){
	  		this.reapt = 2
	  		++this.num
	  		console.log(this.num)
	  		let arr = []
	  		let url = 'http://guoxiao158.top/joke/getjoke.php?page='+this.num
	  		console.log(url)
	  		axios.get(url).then((res)=>{
	  			this.arr = this.arr.concat(res.data.dataList)
	  			this.reapt = 1
	  			console.log(this.arr.concat(res.data.dataList))
 	 			})
			}
	 	})
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3{
	text-align: center;
}
.hello{
	width: 100%;
	height: 100%;
	overflow: auto;
}
</style>
