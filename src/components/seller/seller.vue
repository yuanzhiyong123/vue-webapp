<template>
	<div class="seller" ref="seller">
		<div class="seller-content">
			<div class="overview">
				<h1 class="title">{{seller.name}}</h1>
				<div class="desc border-1px">
					<star :size="36" :score="seller.score"></star>
					<span class="text">({{seller.ratingCount}})</span>
					<span class="text">月售{{seller.sellCount}}单</span>
				</div>
				<ul class="remark">
					<li class="block">
						<h2 class="name">起送价</h2>
						<div class="content"><span>{{seller.minPrice}}</span>元</div>
					</li>
					<li class="block">
						<h2 class="name">商家配送</h2>
						<div class="content"><span>{{seller.deliveryPrice}}</span>元</div>
					</li>
					<li class="block">
						<h2 class="name">平均配送时间</h2>
						<div class="content"><span>{{seller.deliveryTime}}</span>元</div>
					</li>
				</ul>
				<div class="favorite" @click="toggleFavorite">
					<span class="icon-favorite" :class="{'on':favorite}"></span>
					<span class="text">{{textFavorite}}</span>
				</div>
			</div>
			<split></split>
			<div class="bulletin">
				<h1 class="title">公告与活动</h1>
				<p class="content">{{seller.bulletin}}</p>
				<ul class="supports" v-if="seller.supports">
					<li class="support-item" v-for="support in seller.supports">
						<icon :type="support.type" :size="4"></icon><span class="text">{{support.description}}</span>
					</li>
				</ul>
			</div>
			<split></split>
			<div class="pics">
				<h1 class="title">商家实景</h1>
				<div class="pic-wrapper" ref="picWrapper">
					<ul class="pic-list" id="pic-list">
						<li class="pic-item" v-for="pic in seller.pics">
							<img :src="pic">
						</li>
					</ul>
				</div>
			</div>
			<split></split>
			<div class="info">
				<h1 class="title">商家信息</h1>
				<ul class="info-list">
					<li class="info-item" v-for="info in seller.infos">
						{{info}}
					</li>
				</ul>
			</div>
		</div>
	</div>
</template>

<script type="ecmascript-6">
	import {saveToLocal,loadFromLocal} from '../../common/js/store.js';
	import star from '../star/star.vue';
	import split from '../split/split.vue';
	import icon from '../icon/icon.vue';
	import BScroll from 'better-scroll';
	export default{
		props:{
			seller:{
				type:Object
			}
		},
		data(){
			return{
				favorite:(()=>{
					return loadFromLocal(this.seller.id,'favorite',false);
				})()
			}
		},
		components:{
			star,
			split,
			icon
		},
		mounted(){
			this.$nextTick(()=>{
				this._initScroll();
				this._initPicScroll();	
			});
		}, 
		watch:{
			'seller'(){
				this.$nextTick(()=>{
					this._initScroll();
					this._initPicScroll();	
				});
			}
		},
		computed:{
			textFavorite(){
				return this.favorite?'已收藏':'收藏';
			}
		},
		methods:{
			toggleFavorite(){
				this.favorite=!this.favorite;
				saveToLocal(this.seller.id,'favorite',this.favorite);
			},
			_initScroll(){
				if(!this.scroll){
					this.scroll=new BScroll(this.$refs.seller,{
						click:true
					})
				}else{
					this.scroll.refresh();
				}
			},
			_initPicScroll(){
				if(this.seller.pics){
					let picWidth=120;
					let margin=6;
					console.log(this.seller)
					var allWidth=picWidth*this.seller.pics.length+margin*(this.seller.pics.length-1);
					document.getElementById('pic-list').style.width=allWidth+'px';
					if(!this.picScroll){
						this.picScroll=new BScroll(this.$refs.picWrapper,{
							scrollX:true
						});
					}else{
						this.picScroll.refresh();
					}
				}
			}
		}
	}
</script>

<style lang="stylus" rel="stylesheet/stylus">
	@import '../../common/stylus/mixin.styl'
	.seller{
		position: absolute;
		top:174px;
		bottom:0;
		left:0;
		width: 100%;
		overflow: hidden;
		.overview{
			position:relative;
			padding:18px 0;
			margin:0 18px;
			.title{
				margin-bottom:8px;
				line-height: 14px;
				font-size:14px;
				color:rgb(7,17,27);
			}
			.desc{
				padding-bottom: 18px;
				border-1px(rgba(7,17,27,.1));
				font-size:0;
				.star{
					display:inline-block;
					vertical-align: top;
					margin-right:8px;
				}
				.text{
					display:inline-block;
					vertical-align: top;
					margin-right:12px;
					line-height:18px;
					font-size:10px;
					color:rgb(77,85,93);
				}
			}
			.remark{
				display: flex;
				margin-top: 18px;
				.block{
					flex:1;
					border-right:1px  solid rgba(7,17,27,.1);
					text-align: center;
					&:last-child{
						border:none;
					}
					.name{
						margin-bottom: 4px;
						line-height:10px;
						font-size:10px;
						color:rgb(147,153,159);
					}
					.content{
						font-size:10px;
						color:rgb(7,17,27);
						span{
							font-size:24px;
						}
					}
				}
			}
			.favorite{
				position:absolute;
				right:18px;
				top:18px;
				width:36px;
				margin-bottom:4px;
				text-align:center;
				.icon-favorite{
					display:block;
					font-size:24px;
					line-height:24px;
					color:#ccc;
					&.on{
						color:rgb(240,20,20)
					}
				}
				.text{
					font-size: 10px;
					line-height:10px;
					color:rgb(77,85,93);
				}
			}
		}
		.bulletin{
			padding:18px;
			.title{
				margin-bottom:2px;
				line-height: 14px;
				font-size:14px;
				color:rgb(7,17,27);
			}
			.content{
				padding:12px;
				line-height:24px;
				font-size:12px;
				color:rgb(240,20,20);
			}
			.supports{
				.support-item{
					padding:16px; 12px;
					border-top:1px solid rgba(7,17,27,.1);
					.text{
						margin-left: 6px;
						vertical-align:top;
						font-size:12px;
						line-height:16px;
						color:rgb(7,17,27);
					}
				}
			}
		}
		.pics{
			padding:18px ;
			.title{
				margin-bottom:12px;
				line-height: 14px;
				font-size:14px;
				color:rgb(7,17,27);
			}
			.pic-wrapper{
				overflow:hidden;
				.pic-list{
					.pic-item{
						float: left;
						margin-right: 6px;
						width:120px;
						height: 90px;
						img{
							width:120px;
							height: 90px;
						}
						&:last-child{
							margin-right: 0;
						}
					}
				}
			}
		}
		.info{
			padding:18px;
			.title{
				margin-bottom:12px;
				line-height: 14px;
				font-size:14px;
				color:rgb(7,17,27);
			}
			.info-list{
				.info-item{
					padding: 16px 12px;
					line-height:16px;
					font-size:12px;
					color:rgb(7,17,27);
					border-top:1px solid rgba(7,17,27,.1);
				}
			}
		}
	}
</style>