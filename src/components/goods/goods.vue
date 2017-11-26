<template>
    <div class="goods">
    	<div class="menu-wrapper" ref="menuWrapper">
    		<ul class="menu-content">
    			<li class="menu-item border-1px" v-for="(item,index) in goods" :class="{'active':index==currentIndex}" @click="menuSelect(index,$event)">
    				<span class="name"><v-icon :size="3" :type="item.type"></v-icon>{{item.name}}</span>
    			</li>
    		</ul>
    	</div>
    	<div class="foods-wrapper" ref="foodsWrapper">
    		<ul class="foods-content">
    			<li class="foods-list foods-list-hook" v-for="list in goods">
    				<h2 class="title">{{list.name}}</h2>
    				<ul class="foods">
    					<li @click.stop.prevent="selectFood(food,$event)" class="food-item border-1px" v-for="food in list.foods">
    						<div class="icon">
    							<img :src="food.icon">
    						</div>
						<div class="food-content">
							<h3 class="name">{{food.name}}</h3>
							<p class="description">{{food.description}}</p>
							<p class="counts">
								<span class="sellcounts">月售{{food.sellCount}}份</span><span class="rating">好评率{{food.rating}}%</span>
							</p>
							<p class="prices">
								<span class="new">￥{{food.price}}</span>
								<span class="old" v-if="food.oldPrice">￥{{food.oldPrice}}</span>
							</p>
						</div>
						<div class="cartcontrol-wrapper">
							<cartcontrol :food="food" v-on:el="_drop"></cartcontrol>
						</div>
    					</li>
    				</ul>
    			</li>
    		</ul>
    	</div>
    	<food :food="selectedFood" ref="food"></food>
    	<shopcart ref="shopcart" :foodsList="foodsList" :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice"></shopcart>
    </div>
</template>

<script type="ecmascript-6">
import BScroll from 'better-scroll';
import icon from "../icon/icon.vue";
import shopcart from "../shopcart/shopcart.vue";
import cartcontrol from "../cartcontrol/cartcontrol.vue";
import food from "../food/food.vue";
const ERR_OK=0;

    export default{
    	props:{
    		seller:{
    			type:Object
    		}
    	},
    	data(){
    		return{
    			goods:[],
    			listHeight:[],
    			scrollY:0,
    			selectedFood:{}
    		}
    	},
    	created(){
    		this.$http.get('/api/goods').then(function(res){
    			if(res.body.error==ERR_OK){
    				this.goods=res.body.data;
    				this.$nextTick(function(){
		    			this._initScroll();
		    			this._scalHeight();
		    		});
    			}
    		});
    	},
    	computed:{
    		currentIndex(){
    			for(var i=0; i<this.listHeight.length; i++){
    				let height1=this.listHeight[i];
    				let height2=this.listHeight[i+1];
    				if(this.scrollY>=height1&&this.scrollY<height2){
    					return i;
    				}
    			}
    			return 0;
    		},
    		foodsList(){
    			let foods=[];
    			this.goods.forEach(function(good){
    				good.foods.forEach(function(food){
    					if(food.count>0){
    						foods.push(food);
    					}
    				});
    			});
    			return foods;
    		}
    	},
    	methods:{
    		menuSelect(index,event){
    			let foodList=this.$refs.foodsWrapper.getElementsByClassName('foods-list-hook');
    			this.foodScroll.scrollToElement(foodList[index],200);
    		},
    		_initScroll(){
    			this.menuScroll=new BScroll(this.$refs.menuWrapper,{
    				click:true
    			});
		    	this.foodScroll=new BScroll(this.$refs.foodsWrapper,{
		    		probeType:3,
		    		click:true
		    	});
		    	this.foodScroll.on("scroll",(pos)=>{
		    		this.scrollY=Math.abs(Math.round(pos.y));
		    	});
    		},
    		_scalHeight(){
    			let foodList=this.$refs.foodsWrapper.getElementsByClassName('foods-list-hook');
    			let height=0;
    			this.listHeight.push(height);
    			for(var i=0; i<foodList.length; i++){
    				height+=foodList[i].clientHeight;
    				this.listHeight.push(height);
    			}
    		},
    		_drop(el){
    			// this.$refs.shopcart.dropDown(el);
    		},
    		selectFood(food,event){
    			if(!event._constructed){
    				return;
    			}
    			this.selectedFood=food;
    			this.$refs.food.foodShow();
    		}
    	},
    	components:{
    		"v-icon":icon,
    		shopcart,
    		cartcontrol,
    		food
    	}
    }
</script>

<style  lang="stylus" rel="stylesheet/stylus">
	@import "../../common/stylus/mixin.styl"
	.goods{
		display: flex;
		position: absolute;
		width:100%;
		top:174px;
		bottom:46px;
		overflow: hidden;
		.menu-wrapper{
			flex:0 0 80px;
			width:80px;
			background:#f3f5f7;
			.menu-content{
				.menu-item{
					display:table;
					padding:0 12px;
					box-sizing: border-box;
					text-align:center;
					width:100%;
					height: 54px;
					&.active{
						background:#fff;
						.name{
							&:after{
								border-top: none;
							}
						}
					}
					.name{
						display:table-cell;
						vertical-align: middle;
						line-height: 14px;
						width: 56px;
						font-size:12px;
						border-1px(rgba(7,17,27,0.2));
					}
				}
			}
		}
		.foods-wrapper{
			flex:1;
			.foods-content{
				.foods-list{
					.title{
					padding-left: 14px;
					height:26px;
					line-height:26px;
					font-size:12px;
					color:rgb(147,153,159);
					background: #f3f5f7;
					border-left: 3px solid #d9dde1;
				}
				.foods{
					padding:0 18px ;
					.food-item{
						display:flex;
						position:relative;
						padding:18px 0;
						border-1px(rgba(7,17,27,0.1));
						font-size:0;
						.icon{
							flex:0,0,74px;
							width: 74px;
							height:74px;
							vertical-align:top;
							img{
								width:74px;
								height:74px;
							}
						}
						.food-content{
							flex:1;
							vertical-align:top;
							margin-left: 10px;
							font-size:12px;
							.name{
								margin:2px 0 8px 0;
								line-height:14px;
								font-size:14px;
								color:rgb(7,17,27);
							}
							.description{
								margin-bottom:8px;
								font-size:10px;
								line-height:14px;
								color:rgb(147,153,159);
							}
							.counts{
								margin-bottom: 8px;
								line-height:10px;
								font-size:10px;
								color:rgb(147,153,159);
								.sellcounts{
									margin-right:12px;
								}
							}
							.prices{
								font-weight: 700;
								line-height:14px;
								.new{
									font-size: 14px;
									color:rgb(240,20,20);
								}
								.old{
									font-size:10px;
									color:rgb(147,153,159);
									text-decoration:line-through;
								}
							}
						}
						.cartcontrol-wrapper{
							position:absolute;
							right:0;
							bottom: 0;
						}
					}
				}
				}
			}
		}
	}
</style>