<template>
	<div class="pos">
		<el-row>
			<el-col :span='7' class="pos-order" id="order-list">
				<el-tabs>
					<el-tab-pane label="点餐">
						<el-table :data="tableData" border style="width: 100%;">
							<el-table-column prop="goodsName" label="商品名称"></el-table-column>
							<el-table-column prop="count" label="量" width="50"></el-table-column>
							<el-table-column prop="price" label="金额" width="70"></el-table-column>
							<el-table-column label="操作" width="100" fixed="right">

								<template scope="scope">
									<el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
									<el-button type="text" size="small" @click="addOrderList(scope.row)">添加</el-button>
								</template>
							</el-table-column>

						</el-table>
						<div class="total"><small>数量:</small>{{totalCount}} &nbsp;&nbsp;&nbsp;&nbsp;<small>金额:</small>{{totalMoney}}元</div>
						<div class="div-btn">
							<el-button type="warning">挂单</el-button>
							<el-button type="danger" @click="delAllGoods">删除</el-button>
							<el-button type="success" @click="checkout">结账</el-button>
						</div>
					</el-tab-pane>
					<el-tab-pane label="挂单">
						2
					</el-tab-pane>
					<el-tab-pane label="外卖">
						3
					</el-tab-pane>
				</el-tabs>
			</el-col>
			<el-col :span='17'>
				<div class="often-goods">
					<div class="title">常用商品</div>
					<div class="often-goods-list">
						<ul>
							<li v-for="goods in oftenGoods" @click="addOrderList(goods)">
								<span>{{goods.goodsName}}</span>
								<span class="o-price">￥{{goods.price}}元</span>
							</li>
						</ul>
					</div>
				</div>

				<div class="goods-type">
					<el-tabs>
						<el-tab-pane label="汉堡">
							<ul class="cookList">
								<li v-for="goods in type0Goods" @click="addOrderList(goods)">
									<span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>          <span class="foodName">{{goods.goodsName}}</span>          <span class="foodPrice">￥{{goods.price}}元</span>
								</li>
							</ul>
						</el-tab-pane>
						<el-tab-pane label="小食">
							<ul class="cookList">
								<li v-for="goods in type1Goods" @click="addOrderList(goods)">
									<span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>          <span class="foodName">{{goods.goodsName}}</span>          <span class="foodPrice">￥{{goods.price}}元</span>
								</li>
							</ul>
						</el-tab-pane>
						<el-tab-pane label="饮料">
							<ul class="cookList">
								<li v-for="goods in type2Goods" @click="addOrderList(goods)">
									<span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>          <span class="foodName">{{goods.goodsName}}</span>          <span class="foodPrice">￥{{goods.price}}元</span>
								</li>
							</ul>
						</el-tab-pane>
						<el-tab-pane label="套餐">
							<ul class="cookList">
								<li v-for="goods in type3Goods" @click="addOrderList(goods)">
									<span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>          <span class="foodName">{{goods.goodsName}}</span>          <span class="foodPrice">￥{{goods.price}}元</span>
								</li>
							</ul>
						</el-tab-pane>

					</el-tabs>
				</div>
				
			</el-col>
		</el-row>
	</div>
</template>

<script>
	import axios from 'axios'
	export default {
		name: 'Pos',
		data() {
			return {
				tableData: [],
				oftenGoods: [],
				type0Goods: [],
				type1Goods: [],
				type2Goods: [],
				type3Goods: [],
				totalCount: 0,
				totalMoney: 0
			}

		},
		created: function() {
			axios.get('http://jspang.com/DemoApi/oftenGoods.php')
				.then(response => {
					this.oftenGoods = response.data;
					console.log(this.oftenGoods);
				})
				.catch(error => {
					alert("网络错误，不能访问");
				})
			axios.get('http://jspang.com/DemoApi/typeGoods.php')
				.then(response => {
					this.type0Goods = response.data[0];        
					this.type1Goods = response.data[1];        
					this.type2Goods = response.data[2];        
					this.type3Goods = response.data[3];
					console.log(this.type0Goods);
				})
				.catch(error => {
					alert("网络错误，不能访问");
				})
		},
		mounted: function() {
			var orderHeight = document.body.clientHeight;
			console.log(orderHeight);
			document.getElementById('order-list').style.height = orderHeight + 'px';
		},
		methods: {
			//添加订单列表的方法
			      
			addOrderList(goods) {            
				this.totalCount = 0; //汇总数量清0
				            
				this.totalMoney = 0;            
				let isHave = false;             
				//判断是否这个商品已经存在于订单列表
				for(let i = 0; i < this.tableData.length; i++) {
					//console.log(this.tableData[i].goodsId);
					if(this.tableData[i].goodsId == goods.goodsId) { 
				        isHave = true; //存在
					}             
				}            
				//根据isHave的值判断订单列表中是否已经有此商品
				            
				if(isHave) {
				//存在就进行数量添加
					let arr = this.tableData.filter(o => o.goodsId == goods.goodsId);                
					arr[0].count++;
				} else {                
					//不存在就推入数组
					let newGoods = { goodsId: goods.goodsId, goodsName: goods.goodsName, price: goods.price, count: 1 };                
					this.tableData.push(newGoods);             
				} 
				this.getAllMoney();
			},
			//删除单个商品
			delSingleGoods(goods) {        
				console.log(goods);        
				this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId);//数组的过滤 
				this.getAllMoney();
			},
			//删除所有商品
			delAllGoods() {
	            this.tableData = [];            
				this.totalCount = 0;            
				this.totalMoney = 0;
		    },
			//汇总数量和金额
			getAllMoney() {    
					this.totalCount = 0;    
					this.totalMoney = 0;    
					if(this.tableData) {
						this.tableData.forEach((element) => {        
							this.totalCount += element.count;        
							this.totalMoney = this.totalMoney + (element.price * element.count); 
						});    
					}    
				},
			checkout() {    
				if(this.totalCount != 0) {
					this.tableData = [];        
					this.totalCount = 0;        
					this.totalMoney = 0;        
					this.$message({ 
					            message: '结账成功，感谢你又为店里出了一份力!',
					            type: 'success'
		            });     
					} else {
				               this.$message.error('不能空结。老板了解你急切的心情！');
//                             alert("请选餐")
				           } 
				}
		}
	}
</script>

<style scoped>
	.pos-order {
		background-color: #f9fafc;
		border-right: 1px solid #c0ccda;
		height: 100%;
	}
	
	.div-btn {
		margin-top: 10px;
	}
	
	.title {
		height: 20px;
		border-bottom: 1px solid #d3dce6;
		background-color: #f9fafc;
		padding: 10px;
		text-align: left;
	}
	
	.often-goods-list ul li {
		list-style: none;
		float: left;
		border: 1px solid #e5e9f2;
		padding: 10px;
		margin: 10px;
		background-color: white;
	}
	
	.o-price {
		color: #58b7ff;
	}
	
	.goods-type {
		clear: both;
	}
	
	.cookList li {
		list-style: none;
		width: 23%;
		border: 1px solid #E5E9F2;
		height: auto;
		overflow: hidden;
		background-color: #fff;
		padding: 2px;
		float: left;
		margin: 2px;
		cursor: pointor;
	}
	
	.cookList li span {
		float: left;
	}
	
	.foodImg {
		display: block;
		width: 40%;
		float: left;
	}
	
	.foodName {
		padding-left: 10px;
		font-size: 18px;
		color: brown;
	}
	
	.foodPrice {
		font-size: 16px;
		padding-left: 10px;
		padding-top: 10px;
		display: block;
	}
	
	.total {
		height: 30px;
		line-height: 35px;
	}
</style>