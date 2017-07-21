<template>
	<div class="left-nav">
		<ul>
			<li>
				<i class="icon iconfont icon-shouye2"></i>
				<div>收银222</div>
			</li>
		    <li>
		    	<i class="icon iconfont icon-accountfilling"></i>
		    	<div>店铺</div>
		    </li>
		    <li>
		    	<i class="icon iconfont icon-cart"></i>
		    	<div>商品</div>
		    </li>
		    <li>
		    	<i class="icon iconfont icon-shouye1"></i>
		    	<div>会员</div>
		    </li>
		    <li>
		    	<i class="icon iconfont icon-shouye"></i>
		    	<div>统计</div>
		    </li>
		</ul>
		<div class="Hello">
			<router-link to="./Hello">Hello</router-link>
			<router-link to="./">Pos</router-link>
		</div>
		
	</div>
</template>

<script>
    export default{
		name:'leftNav'
	}
</script>

<style>
	.left-nav{
		color: #fff;
		font-size: 10px;
		height: 100%;
		background-color: #1D8ce0;
		float: left;
		width: 5%;
	}
	.iconfont{
		font-size: 24px;
	}
	.left-nav ul{
		padding: 0px;
		margin: 0px;
	}
	.left-nav ul li{
		list-style: none;
		text-align: center;
		border-bottom:1px solid #2C3E50;
		padding: 10px;
	}
	.Hello{
		color: black;
		font-size: 20px;
	}
</style>