<template>
	<div class="shopcart" @click.prevent.stop="carListShowToggle">
		<div class="content">
			<div class="content-left">
				<div class="logo-wrapper">
					<div class="logo" :class="{'active':totalCount>0}">
						<span class="icon-shopping_cart"></span>
					</div>
					<div class="num" v-show="totalCount>0">{{totalCount}}</div>
				</div>
				<div class="price">{{totalPrice}}元</div>
				<div class="desc">另需配送费￥{{deliveryPrice}}</div>
			</div>
			<div class="content-right">
				<div class="pay_1 pay" v-show="totalPrice<=0">￥{{minPrice}}元起送</div>
				<div class="pay_2 pay" v-show="totalPrice>0&&totalPrice<minPrice">还差{{minPrice-totalPrice}}起送</div>
				<div class="pay_3 pay" v-show="totalPrice>=minPrice" @click.prevent.stop="goPay">去结算</div>
			</div>
			<div class="ball-container">
				<div class="ball" v-for="ball in balls" v-show="ball.show" key="ball"></div>
			</div>
			<div class="cartlist"  v-show="isCarListShow" v-if="foodsList.length>0">
				<div class="cartlist-header">
					<h3 class="title">购物车</h3>
					<div class="clearshop" @click.prevent.stop="clearShop">清空</div>
				</div>
				<div class="cartlist-content" ref="cartlistContent">
					<ul  >
						<li class="cartlist-item border-1px" v-for="food in foodsList">
							<span class="name">{{food.name}}</span>
							<span class="price">{{food.price*food.count}}</span>
							<div class="cartcontrol-wrapper">
								<cartcontrol :food="food"></cartcontrol>
							</div>
						</li>
				</ul>
				</div>
				<div class="mask" v-show="isCarListShow"></div>
			</div>
		</div>
	</div>
</template>

<script type="ecmascript-6">
	import cartcontrol from "../cartcontrol/cartcontrol.vue";
	import BScroll from "better-scroll";
	export default{
		props:{
			deliveryPrice:{
				type:Number
			},
			minPrice:{
				type:Number
			},
			foodsList:{
				type:Array
			}
		},
		data(){
			return{
				balls:[
					{show:false},
					{show:false},
					{show:false},
					{show:false},
					{show:false}
				],
				dropBalls:[],
				isCarListShow:false
			}
		},
		computed:{
			totalPrice(){
				let price=0;
				this.foodsList.forEach(function(food){
					price+=food.price*food.count;
				});
				return price;
			},
			totalCount(){
				let count=0;
				this.foodsList.forEach(function(food){
					count+=food.count;
				});
				return count;
			}
		},
		methods:{
			carListShowToggle(){
				if(this.foodsList.length==0){
					return;
				}
				this.isCarListShow=!this.isCarListShow;
				if(this.isCarListShow){
					this.$nextTick(()=>{
						this.cartListWrapper=new BScroll(this.$refs.cartlistContent,{
						click:true
						});
					});
				}
				console.log(22)
			},
			clearShop(){
				this.foodsList.forEach(function(food){
					food.count=0;
				});
				this.isCarListShow=false;
			},
			goPay(){
				console.log(11)
				// window.alert("功能尚未实现！");
			}
		},
		components:{
			cartcontrol
		}
	};
</script>

<style lang="stylus" rel="stylesheet/stylus">
	@import "../../common/stylus/mixin.styl";
	.shopcart{
		position:fixed;
		left:0;
		bottom:0;
		z-index:50;
		height: 48px;
		width:100%;
		background: #000;
		.content{
			display: flex;
			background:#141d27;
			.content-left{
				flex:1;
				font-size:0;
				.logo-wrapper{
					display:inline-block;
					position: relative;
					top:-10px;
					z-index:50;
					margin:0 12px;
					padding:6px;
					width:56px;
					height:56px;
					box-sizing: border-box;
					vertical-align:top;
					border-radius:50%;
					background:#141d27;
					text-align: center;
					.logo{
						width:100%;
						height:100%;
						border-radius: 50%;
						background: #2b343c;
						.icon-shopping_cart{
							font-size:24px;
							color:#80808a;
							line-height: 44px;
						}
						&.active{
							background:rgb(0,160,220);
							.icon-shopping_cart{
								color:#fff;
							}
						}
					}
					.num{
						position:absolute;
						top:0;
						right:0;
						padding:3px 8px;
						border-radius:16px;
						font-size:9px;
						font-weight:700;
						color:#fff;
						background:#f00;
					}
				}
				.price{
					display:inline-block;
					vertical-align:top;
					margin-top:12px;
					padding-right:12px;
					border-right:1px solid rgba(255,255,255,.1);
					font-size:16px;
					line-height:24px;
					font-weight: 700;
					color:rgba(255,255,255,.4);
				}
				.desc{
					display:inline-block;
					vertical-align:top;
					margin:12px 0 0 12px;
					line-height:24px;
					font-size:12px;
					color:rgba(255,255,255,.4);
				}
			}
			.content-right{
				flex:0 0 105px;
				width:105px;
				.pay{
					height:48px;
					width:105px;
					line-height:48px;
					text-align:center;
					font-size:12px;
					font-weight:700;
					color:rgba(255,255,255,.4);
					&.pay_1{
						background:#2b343c;
					}
					&.pay_2{

					}
					&.pay_3{
						background:#31e16f;
						color:#fff;
					}
				}
			}
			.ball-container{
				.ball{
					position:fixed;
					left:40px;
					bottom:30px;
					width:16px;
					height:16px;
					border-radius:50%;
					background:rgb(0,160,220);
					transition:1s all cubic-bezier(0.49, -0.29, 0.75, 0.41);
				}
			}
			.cartlist{
				position:absolute;
				left:0;
				bottom:48px;
				z-index: -1;
				width:100%;
				max-height: 257px;
				.cartlist-header{
					position:relative;
					height:40px;
					padding-left: 18px;
					background:#f3f5f7;
					border-bottom:2px solid rgba(7,17,27,.1);
					.title{
						line-height:40px;
						font-size:14px;
						color:rgb(7,17,27);
					}
					.clearshop{
						position:absolute;
						right:18px;
						top:14px;
						font-size: 12px;
						color:rgb(0,160,220);
					}
				}
				.cartlist-content{
					padding:0 18px;
					max-height:217px;
					background:#fff;
					overflow: hidden;
					.cartlist-item{
						position:relative;
						height:48px;
						border-1px(rgba(7,17,27,.1));
						.name{
							line-height:48px;
							font-size:14px;
							color:rgb(7,17,27);
						}
						.price{
							position:absolute;
							top:17px;
							right:100px;
							font-size:14px;
							color:rgb(240,20,20);
						}
						.cartcontrol-wrapper{
							position:absolute;
							right:0;
							top:14px;
						}
					}
				}
				.mask{
					position:fixed;
					left:0;
					top:0;
					bottom: 0;
					right: 0;
					z-index:-2;
					background:rgba(0,0,0,0.7)
				}
			}
		}
	}
</style>