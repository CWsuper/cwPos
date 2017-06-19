<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <ul>
    	<li v-for="item in newdata">
    		<span>姓名：{{item.name}}</span>
    		<span>成绩：{{item.value}}</span>
    	</li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios'
//import data from "../../util/mock";
 
export default {
  name: 'hello',
  data () {
    return {
      msg: 'mock.js用法',
      newdata:[
      {
      	name:"cheng",
      	value:100
      },
      {
      	name:"wang",
      	value:75
      },
      {
      	name:"li",
      	value:90
      },
      {
      	name:"yan",
      	value:85
      }
      ]
    }
  }
//created:function(){
//	axios.get('http://g.cn')
//    .then((data)=>{
//    	this.newdata = data;
//    })
//    .watch((error)=>{
//    	$message.error("错误");
//    })
//}
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  /*display: inline-block;*/
  text-align: left;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
