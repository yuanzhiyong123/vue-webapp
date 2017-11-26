<template>
	<div class="select-rating">
		<div class="select-type border-1px">
			<span class="block positive" :class="{'active':ratingType2==0}" @click="all(0,$event)">{{deac.all}}<span class="count">10</span></span>
			<span class="block positive" :class="{'active':ratingType2==1}" @click="positive(1,$event)">{{deac.positive}}<span class="count">10</span></span>
			<span class="block negative" :class="{'active':ratingType2==2}" @click="negative(2,$event)">{{deac.negative}}<span class="count">10</span></span>
		</div>
		<div class="onlycontent" :class="{'on':isOnlyContent}" @click="toggleOnly">
			<span class="icon-check_circle"></span>
			<span class="text">只看有内容的评价</span>
		</div>
	</div>
</template>

<script type="ecmascript-6">
	const ALL=0;
	const POSITIVE=1;
	const NEGATIVE=2;
	export default{
		props:{
			ratingType:{
				type:Number,
				default: ALL
			},
			onlyContent:{
				type:Boolean
			},
			deac:{
				type:Object,
				default(){
					return {
						"all":"全部",
						"positive":"满意",
						"negative":"不满意"
					};
				}
			},
			ratings:{
				type:Array,
				default(){
					return [];
				}
			}
		},
		data(){
			return{
				isOnlyContent:this.onlyContent,
				ratingType2:0
			}
		},
		methods:{
			toggleOnly(){
				this.isOnlyContent=!this.isOnlyContent;
			},
			all(type,event){
				// if(!event._construted){
				// 	return;
				// }
				this.ratingType2=type;
			},
			positive(type,event){
				// if(!event._construted){
				// 	return;
				// }
				this.ratingType2=type;
			},
			negative(type,event){
				// if(!event._construted){
				// 	return;
				// }
				this.ratingType2=type;
			},
		}
	};    
</script>

<style lang="stylus" rel="stylesheet/stylus">
	@import "../../common/stylus/mixin.styl"
	.select-rating{
		padding-top: 18px;
		.select-type{
			margin:0 18px;
			padding-bottom: 18px;
			border-1px(rgba(7,17,27,.1));
			font-size:0;
			.block{
				display: inline-block;
				padding:8px 12px;
				margin-right: 8px;
				font-size:12px;
				color:rgb(77,85,93);
				&.active{
					.count{
						color:#fff;
					}
				}
				.count{
					font-size: 8px;
					margin-left: 2px
				}
				&.positive{
					background: rgba(0,160,220,.2);
					&.active{
						color:#fff;
						background: rgb(0,160,220);
					}
				}
				&.negative{
					background:rgba(77,85,93,.2);
					&.active{
						color:#fff;
						background: rgb(77,85,93,.2);
					}
				}
			}
		}
		.onlycontent{
			padding:12px 18px;
			border-bottom: 2px solid rgba(7,17,27,.1);
			font-size:0;
			&.on{
				.icon-check_circle{
					color:green;
				}
			}
			.icon-check_circle{
				display:inline-block;
				vertical-align: top;
				font-size:24px;
				color:rgb(147,153,159);
			}
			.text{
				display:inline-block;
				margin-left:4px;
				vertical-align:top;
				line-height: 24px;
				font-size:12px;
				color:rgb(147,153,159);
			}
		}
	}
</style>