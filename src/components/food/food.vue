<template>
<transition name="move">
	<div class="food" v-show="showFlag" ref="food">
    		<div class="food-content">
		    	<div class="banner">
		    		<img :src="food.image" width="100%" height="100%">
		    		<span class="icon-arrow_lift" @click="_isHide"></span>
		    	</div>
		    	<div class="data">
		    		<h2 class="title">{{food.name}}</h2>
		    		<div class="ratings">
		    			<span class="sellCount">月售{{food.sellCount}}份</span>
		    			<span class="rating">好评率{{food.rating}}%</span>
		    		</div>
		    		<div class="price">
		    			<span class="new">￥{{food.price}}</span>
		    			<span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
		    		</div>
		    		<div class="cart-wrapper">
		    			<div class="cartshop" v-show="!food.count||food.count<1" @click="_addFood">加入购物车</div>
		    			<div class="cartcontrol-wrapper" v-show="food.count>0">
		    				<cartcontrol :food="food"></cartcontrol>
		    			</div>
		    		</div>
		    	</div>
		    	<split v-if="food.info"></split>
		    	<div class="introduce" v-if="food.info">
		    		<h2 class="title">商品介绍</h2>
		    		<p class="text">{{food.info}}</p>
		    	</div>
		    	<split></split>
		    	<div class="food-rating">
		    		<div class="title">商品评价</div>
		    		<selectrating :ratingType="ratingType" :onlyContent="onlyContent" :deac="deac" :ratings="food.ratings"></selectrating>
		    		<ul class="rating-all" v-show="food.ratings">
		    			<li class="rating-item border-1px" v-for="rating in food.ratings">
		    				<div class="time">{{rating.rateTime}}</div>
		    				<div class="rating-conten">
		    					<span class="icon" :class="{'icon-thumb_down':rating.rateType==1,'icon-thumb_up':rating.rateType==0}"></span>
		    					<span class="text">{{rating.text}}</span>
		    				</div>
		    				<div class="user">
		    					<span class="username">{{rating.username}}</span>
		    					<span class="avatar">
		    						<img :src="rating.avatar" width="12" height="12">
		    					</span>
		    				</div>
		    			</li>
		    		</ul>
		    	</div>
    		</div>
    	</div>
</transition>
</template>

<script type="ecmascript-6">
	import Vue from "vue";
	import BScroll from "better-scroll";
	import cartcontrol from "../cartcontrol/cartcontrol.vue";
	import split from "../split/split.vue";
	import selectrating from "../selectrating/selectrating.vue";

	const ALL=0;
	const POSITIVE=1;
	const NEGATIVE=2;
export default{
	props:{
		food:{
			type:Object
		}
	},
	data(){
		return{
			showFlag:false,
			ratingType:ALL,
			onlyContent:false,
			deac:{
				"all":"全部",
				"positive":"推荐",
				"negative":"吐槽"
			}
		}
	},
	methods:{
		foodShow(){
			this.showFlag=true;
			this.ratingType=ALL;
			this.onlyContent=false;
			this.$nextTick(()=>{
				this.foodWrapper=new BScroll(this.$refs.food,{
					click:true
				});
			});
		},
		_addFood(){
			if(!this.food.count){
				Vue.set(this.food,"count",1);
			}else{
				this.food.count++;
			}
		},
		_isHide(){
			this.showFlag=false;
		}
	},
	components:{
		cartcontrol,
		split,
		selectrating
	}
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
	@import "../../common/stylus/mixin.styl"
	.food{
		position: fixed;
		left:0;
		top:0;
		right:0;
		bottom:48px;
		z-index: 30;
		background: #fff;
		opacity:1;
		transform:scale3d(1,1,1);
		&.move-enter-active,&.move-leave-active{
			transition: .5s all;
		}
		&.move-enter,&.move-leave-to{
			transform:scale3d(0.4,0.4,0.4);
			opacity: 0;
		}
		.banner{
			position:relative;
			width: 100%;
			padding-top:100%;
			img{
				position:absolute;
				left:0;
				top:0;
			}
			.icon-arrow_lift{
				position:absolute;
				padding:8px;
				top:10px;
				left: 10px;
				font-size:20px;
				color:#fff;
			}
		}
		.data{
			position:relative;
			padding:18px;
			.title{
				margin-bottom:8px;
				line-height:14px;
				font-size:14px;
				color:rgb(7,17,27);
				font-weight: 700;
			}
			.ratings{
				margin-bottom: 18px;
				font-size:0;
				.sellCount{
					line-height:10px;
					font-size:10px;
					color:rgb(147,153,159);
				}
				.rating{
					margin-left: 12px;
					line-height:10px;
					font-size:10px;
					color:rgb(147,153,159);
				}
			}
			.price{
				.new{
					line-height: 24px;
					font-size:14px;
					font-weight:700;
					color:rgb(240,20,20);
				}
				.old{
					line-height:24px;
					font-size:10px;
					font-weight:700;
					color:rgb(147,153,159);
				}
			}
			.cart-wrapper{
				position:absolute;
				bottom:18px;
				right:18px;
				.cartshop{
					width:74px;
					height:24px;
					line-height:24px;
					text-align:center;
					font-size:10px;
					color:#fff;
					background:rgb(0,160,220);
					border-radius:12px;
				}
				.cartcontrol-wrapper{
					position: relative;
    					top: 9px;
				}
			}
		}
		.introduce{
			padding:18px;
			.title{
				margin-bottom:6px;
				line-height:14px;
				font-size:14px;
				color:rgb(7,17,27);
			}
			.text{
				padding:0 8px;
				line-height: 24px;
				font-size:12px;
				color:rgb(77,85,93);

			}
		}
		.food-rating{
			padding-top:18px;
			.title{
				margin-left:18px;
				line-height:14px;
				font-size:14px;
				color:rgb(7,17,27);
			}
			.rating-all{
				padding:0 18px;
				.rating-item{
					position:relative;
					padding:16px 0;
					border-1px(rgba(7,17,27,.1));
					.time{
						margin-bottom:6px;
						line-height: 12px;
						font-size: 10px;
						color:rgb(147,153,159);
					}
					.rating-conten{
						font-size:0;
						.icon{
							display: inline-block;
							vertical-align: top;
							font-size:12px;
							line-height:16px;
							&.icon-thumb_up{
								color:rgb(0,160,220);
							}
							&.icon-thumb_down{
								color: rgb(147,153,159);
							}
						}
						.text{
							display:inline-block;
							margin-left:4px;
							vertical-align:top;
							line-height:16px;
							font-size:12px;
							color:rgb(7,17,27);
						}
					}
					.user{
						position:absolute;
						top:16px;
						right: 0;
						.username{
							line-height:12px;
							font-size:10px;
							color:rgb(147,153,159);
							margin-right: 6px;
						}
						.avatar{
							width: 12px;
							height:12px;
							img{
								border-radius:50%;
							}
						}
					}
				}
			}
		}
	}
</style>