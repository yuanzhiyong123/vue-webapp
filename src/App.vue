<template>
  <div>
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="seller">商家</router-link>
      </div>
    </div>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script>
import Header from "./components/header/header.vue";
import {urlParse} from './common/js/util.js';
const ERR_OK=0;
export default {
      data(){
      	return{
      		seller:{
      			id:(()=>{
      				return urlParse().id;
      			})()
      		}
      	}
      },
      created(){
		this.$http.get("/api/seller?id="+this.seller.id).then(function(res){
			if(res.body.error===ERR_OK){
				this.seller=Object.assign({},this.seller,res.body.data);
			}	
		})
	},
     components:{
     		"v-header":Header
  	}
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "./common/stylus/mixin.styl"
  #app
    .tab
      display:flex
      height:40px
      line-height:40px
      border-1px(rgba(7,17,27,0.2))
      .tab-item
        flex:1
        text-align:center
        &>a
          display:block
          font-size:14px
          color:rgb(77,85,93)
          line-height:40px
          &.active
            color:rgb(240,20,20)
</style>
