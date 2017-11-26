<template>
	<div class="cartcontrol">
		<transition name="fade">
			<div class="remove icon-remove_circle_outline" v-show="food.count>0" @click.prevent.stop="countRemove"></div>
		</transition>
		<div class="count" v-show="food.count>0">{{food.count}}</div>
		<div class="add icon-add_circle" @click.prevent.stop="counAdd($event)"></div>
	</div>
</template>

<script type="ecmascript-6">
	import Vue from "vue";
	export default{
		props:{
			food:{
				type:Object
			}
		},
		methods:{
			counAdd(event){
				if(this.food.count){
					this.food.count++;
				}else{
					Vue.set(this.food,'count',1);
				}
				this.$emit('el',event.target);
			},	
			countRemove(){
				this.food.count--;
			}
		}
	}
</script>

<style lang="stylus" rel="stylesheet/stylus">
	.cartcontrol{
		font-size:0;
		line-height: 24px;
		color:rgb(0,160,220)
		.remove{
			display: inline-block;	
			font-size:24px;
			opacity: 1;
			transform:translate3D(0,0,0) rotate(0deg);
			&.fade-enter-active,&.fade-leave-active{
				transition: .3s all;
			}
			&.fade-enter,&.fade-leave-to{
				opacity: 0;
				transform:translate3D(30px,0,0) rotate(360deg);
			}
		}
		.add{
			display: inline-block;	
			font-size:24px;
		}
		.count{
			display:inline-block;
			vertical-align:top;
			width:24px;
			height: 24px;
			line-height:24px;
			text-align: center;
			font-size:10px;
			color:rgb(147,153,159);
		}
	}
</style>