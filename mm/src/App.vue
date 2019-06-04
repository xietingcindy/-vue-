<template>
  <div id="app">
    <div class="head">
      <v-header :seller="seller"></v-header>
    </div>
    <div class="tab border-1px">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script>
import header from './components/header/header.vue'
const ERR_OK=0;
export default {
  // name: 'App',
  // components: {},
  // name: 'header',
  data(){
    return {
      seller: {}
    };
  },
  created () {
    this.$http.get('/api/seller')
      .then(response => {
        console.log(response);
        response=response.body
        if(response.errno===ERR_OK){
          this.seller = response.data
          //console.log(this.seller)
          }
        });  
  },
  components: {
   'v-header':header,
  }
}
</script>

<style lang="stylus">
@import "./common/stylus/mixin.styl"
  #app
   .tab
    display flex
    width 100%
    height 40px
    line-height 40px
  
  .tab-item
    flex 1
    text-align center
    // border-bottom: 1px solid rgba(7, 17, 27, 0.1)
    border-1px(rgba(7, 17, 27, 0.1))
  &>a
    display  block
    font-size 14px
    color rgb(77, 85, 93)  
    &.active
      color rgb(240, 20, 20)
  
</style>


