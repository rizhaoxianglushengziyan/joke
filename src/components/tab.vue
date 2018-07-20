<template>
  <div class="hello">
 			<h3>详情评论</h3>
 			<div>
	 		<p>{{arr.content}}</p>
	 		</div>
	 		<div>评论:</div>
	 		<div v-for="val,index in list">
	 			<p>{{val.content}}</p>
	 		</div>
	 		<div>
	 			<input type="text" v-model="query"/><button @click="add(arr.id)">添加评论</button>
	 		</div>
  </div>
</template>

<script>
import axios from 'axios'
import QS from 'qs'
export default {
  data () {
    return {
      arr:[],
      list:[],
      query:''
    }
  },
  created(){
//	console.log(this.$route.query.id)
  	let aa=this.$route.query.id
  	var that =this
		axios.get('http://guoxiao158.top/joke/getjoke.php').then((res)=>{
			this.arr=res.data.dataList[aa]
		  console.log(res.data.dataList[aa])
			let bb=this.arr.id
//				console.log(bb)
			axios.get('http://guoxiao158.top/joke/getpl.php?id='+bb).then((res)=>{
				that.list=res.data.dataList
		  	console.log(res.data.dataList)
		  	
			})
				
		})
  },
  methods:{
  	add(i){
  		console.log(i)
				var that=this;
  		 let loginParams = {
         	uid:i,
         	pinglun:this.query,
         };
  			axios.post('http://guoxiao158.top/joke/addpl.php',QS.stringify(loginParams),{
		  	headers: {
            'Content-Type': 'application/x-www-form-urlencoded'
          }}).then(function(res){
//        	location.reload()
       		console.log(res)
       		let aa=that.$route.query.id
						axios.get('http://guoxiao158.top/joke/getjoke.php').then((res)=>{
							that.arr=res.data.dataList[aa]
						  console.log(res.data.dataList[aa])
							let bb=that.arr.id
				//				console.log(bb)
							axios.get('http://guoxiao158.top/joke/getpl.php?id='+bb).then((res)=>{
								that.list=res.data.dataList
						  	console.log(res.data.dataList)
						  	
							})
								
						})
       })
  	}
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3{
	text-align: center;
}
</style>
