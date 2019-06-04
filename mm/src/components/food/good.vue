<template>
    <transition name="mv">
    <div class="food" v-show="showFlag" ref="good1">
        <div class="food-content">
            <div class="image-header">
                <img class="img1" :src="good.image">
                <div class="back" @click="hide">
                    <i class="icon-arrow_lift"></i>
                </div>
            </div>
            <div class="content">
                <h1 class="tittle">{{good.name}}</h1>
                <div class="detail1">
                    <span class="sell-count">月售{{good.sellCount}}份</span>
                    <span class="rating">好评率{{good.rating}}%</span>
                </div>
                <div class="price">
                    <span class="now">￥{{good.price}}</span>
                    <span class="old" v-show="good.oldPrice">￥{{good.oldPrice}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                <!-- 绑定前为子组件属性，后为父组件 -->
                    <cartcontrol :food="good"></cartcontrol>
                </div>
                <transition name="fade2">
                 <div v-show="!good.count||good.count===0" @click="addFirst($event)" class="buy">加入购物车</div>           
                 </transition>
                
            </div>
            <split v-show="good.info"></split>
            <div class="info" v-show="good.info">
                <h1 class="tittle">商品信息</h1>
                <p class="text">{{good.info}}</p>
            </div>
            <split v-show="good.info"></split>
            <div class="rating1">
                <h1 class="tittle">商品评价</h1>
                <ratingselect :select-type="selectType" :only-content="onlyContent"
                :desc="desc" :ratings="good.ratings" @typeSelect="update1($event)" @contentToggle="update2($event)"></ratingselect>
            </div>
        </div>
    </div>
    </transition>
</template>

<script>
import BScroll from 'better-scroll'
import Vue from 'vue'
import cartcontrol from '../cartcontrol/cartcontrol.vue'
import split from '../split/split.vue';
import ratingselect from '../ratingselect/ratingselect.vue'

const  POSITIVE=0;
const NAGATIVE=1;
const ALL=2;
export default {
    props:{
        good:{
            type:Object,
             default(){
                return {};
            }
        },
        selectType:ALL,
        onlyContent:true,
    },
    //依赖
    data(){
        return {
            showFlag:false,
            // selectType:ALL,
            // onlyContent:true,
            desc:{
                all:'全部',
                positive:'推荐',
                nagative:'吐槽'
            }          
        }
    },
    methods:{
        show(){
            this.showFlag=true;
            // this.selectType=ALL;
            // this.onlyContent=true;
            this.$nextTick(()=>{
                if(!this.$scroll){
                    this.scroll=new BScroll(this.$refs.good1,{click:true});
                }
                this.scroll.refresh();              
            })
        },
        update1(type){
            this.selectType=type;
        },
        update2(onlyContent){
            this.onlyContent=onlyContent;
        },
        hide(){
             this.showFlag=false;
        },
        addFirst($event){
            if(!event._constructed){
                  return;
            }
            if(!this.good.count){
          
                Vue.set(this.good,'count',1);               
            }
            else{
                this.good.count++;
            }
        }
    },
    components:{
        cartcontrol,
        split,
        ratingselect
    }
}
</script>

<style lang="stylus">
.food
    position fixed
    left 0
    top 0
    bottom 48px
    z-index 30
    width 100%
    background #ffffff  
    .mv-enter-active,.mv-leave-active 
        transition  all .2s linear       
        transform translate3d(0,0,0)          
    .mv-enter,.mv-leave
        transform translate3d(100%,0,0)
     .food-content  
        .image-header
            position relative
            width 100%
            height 0
            // width 512px
            // height 384px
            padding-top 100%          
            .img1
                position absolute
                top 0
                left 0           
                width 100%
                height 100%
            .back
                position absolute
                top 10px
                left 0
                .icon-arrow_lift
                    display block
                    padding 10px
                    font-size 20px
                    color #fff
        .content
            position relative
            padding 18px
            .tittle
                line-height 14px
                margin-bottom 8px
                font-size 14px
                font-weight 700
                color rgb(7,17,27)
            .detail1
                margin-bottom 18px
                line-height 10px
                font-size 0
                height 10px
                .sell-count,.rating
                    font-size 10px
                    color rgb(147,153,159)
                .sell-count
                    margin-right 12px
            .price 
                font-weight 700
                line-height 24px
                .now
                    margin-right 8px
                    font-size 14px
                    color rgb(240,20,20)
                .old
                    text-decoration line-through
                    font-size 10px
                    color rgb(147,153,159)
            .cartcontrol-wrapper
                position absoulute
                right 12px
                bottom 12px
            .buy
                position absolute 
                right 18px
                top 100px
                z-index 10  
                height 24px
                line-height 24px
                padding 0 12px
                box-sizing border-box
                font-size 10px
                border-radius 12px
                color #ffffff
                background rgb(0,160,220)
            
                .move-enter-active, .move-leave-active
                    opacity 1
                    transition all 0.5s
                .move-enter, .move-leave-active 
                    opacity 0      
                    background rgb(0,160,220)
        .info
            padding 18px
            .tittle
                line-height 14px
                margin-bottom 6px
                font-size 14px
                color rgb(7,17,27)
            .text
                line-height 24px
                padding 0 8px 
                font-size 12px
                color rgb(77,85,93)
        .rating1
            padding-top 18px
            .tittle
                line-height 14px
                margin-left 18px
                font-size 14px
                color rgb(7,17,27)
       


</style>


