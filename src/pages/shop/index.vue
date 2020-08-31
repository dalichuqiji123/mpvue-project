<template>
    <view class='shop'>
        <navbar />
        <!-- 横向tab -->
        <view class="scroll">
            <scroll-view 
                :scroll-x="true" 
                :scroll-into-view="toView" 
                :scroll-with-animation='true'
                class="scroll-view"
            >
            <view 
                v-for='item in cityArr'
                :id="'k'+item.id"
                :key='item.id'
                v-text='item.name'
                class="sc-city"
                :class="onView=='k'+item.id && 'on'"
                @tap="scrollChange(item.id)"
            >
            </view>
            
            </scroll-view>
        </view>
        <!-- 商品详情 -->
        <view class='shop-detail'>
            <view class='sd-left'>
                <view class="sdl-top">
                    <img :src="img.searchIcon">
                    <text>09:00~22:00</text>
                </view>
                <view class="sdl-bottom">
                    <img :src="img.searchIcon">
                    <text @tap='go'>深圳市宝安区千锋教育</text>
                </view>
            </view>
            <view class='sd-right'>
                <view class="sdr-box">
                    <image :src='img.searchIcon' />
                    <text @tap='call'>电话</text>
                </view>
            </view>
        </view>
        <!-- Tab列表 -->
        <view class="tab" >
            <view class="tab-top">
                <view :class='count==0 && "on"' @tap="tabChange(0)">商品</view>
                <view :class='count==1 && "on"' @tap="tabChange(1)">店铺</view>
                <view :class='count==2 && "on"' @tap="tabChange(2)">活动</view>
            </view>
            <swiper 
                :style='{"height":height+"rpx"}'
                class="tab-swiper"
                :current="count"
                @change='swiperChange'
                > 
                <swiper-item>
                    <view>商品列表（使用接口调数据然后循环）</view>
                </swiper-item>
                <swiper-item>
                    <view>活动列表（使用接口调数据然后循环）</view>
                </swiper-item>
                <swiper-item>
                    <view>店铺品牌信息</view>
                </swiper-item>
            </swiper>
        </view>
        
    </view>

</template>
<script>
import navbar from '@/components/navbar'
import img from '@/utils/img'
export default {
    components : {
        navbar
    },
    data:function(){
        return {
            img,
            location : false,
            count:0,
            height:0,
            cityArr: [
                {id: 1, name:'深圳'},
                {id: 2, name:"北京"},
                {id: 3, name:"上海"},
                {id: 4, name:'重庆'},
                {id: 5, name:'南昌'},
                {id: 6, name:'广州'},
                {id: 7, name:'杭州'},
                {id: 8, name:'武汉'},
                {id: 9, name:'天津'},
                {id: 10, name:'长沙'},
                {id: 11, name:'石家庄'}
            ],
            toView:'k1',//控制滚动的走向
            onView:'k2'//控制tab的样式
        }
    },
    methods:{
        call(){
            mpvue.makePhoneCall({
                phoneNumber: '13479246656' //仅为示例，并非真实的电话号码
            })
        },
        go(){
            let that = this
           if(that.location){
            //    如果loaction那么就执行toMap()函数
               that.toMap()
           }else{
            //    不然就让用户通过手动授权获取地理位置
               mpvue.openSetting({
                   success(){
                       that.toMap()
                   }
               })
           }  
        },
        toMap(){
            // 获取地理信息
            mpvue.getLocation({
                type: 'gcj02',
                success(res){
                       let lat=res.latitude
                       let log=res.longitude
                    // 根据上面的定位获取经纬度，然后通过这个经纬度打开地图并标明位置信息
                       mpvue.openLocation({
                           latitude:lat,//一般指的是店铺的位置
                           longitude:log,
                           scale: 18
                       })
                   }
            })
        },
        tabChange(tab){
            this.count=tab
            this.calcHeight(tab)
        },
        swiperChange(e){
            this.count=e.target.current
            this.calcHeight(e.target.current)
        },
        calcHeight(idx){
            let p=0
            switch(idx){
                case 0 :
                    p=500
                    break;
                case 1 :
                    p=200*3
                    break;
                case 2 :
                    p=300
                    break;
                default:
            }
            this.height=p
        },
        scrollChange(id){
            this.onView="k"+id
            this.toView="k"+(id-2)
        }
    },
    onShow(){
        var that = this
        // 获取授权信息
        mpvue.getSetting({
        // 成功获取授权信息
          success(res){
            //   如果地理授权成功那么把location设置为ture
              if(res.authSetting['scope.userLocation']){
                  that.location=true
              }else{
                //否则就弹框使用户授权获取地理位置
                  mpvue.authorize({
                    scope: 'scope.userLocation',
                    success(){
                        that.location=true
                    },
                    fail(){
                        that.location=false
                    }
                  })
              }
          } 
        })
        that.calcHeight(0)
    }
}
</script>
<style >
/* 店铺详情 */
.shop-detail{
    width:690rpx;
    height:128rpx;
    margin:20rpx auto 0;
    border-top:1rpx solid rgba(221,221,221,1);
    border-bottom:1rpx solid rgba(221,221,221,1);
    display:flex;
}
.sd-left{
    flex:462;
    line-height: 64rpx;
    border-right:1px solid #ccc;
}
.sd-left img{
    display: inline-block;
  width: 32rpx;
  height: 32rpx;
  vertical-align: middle;
}
.sd-right{
    flex:230;
}
.sd-right img{
    display: inline-block;
  width: 32rpx;
  height: 32rpx;
  vertical-align: middle;
}
.sdr-box{
     width: 124rpx;
    height: 50rpx;
    margin-top: 39rpx;
    margin-right: 20rpx;
    margin-left :40rpx;
    border-radius: 25rpx;
    background-color: #E33C49;
    color: #ffffff;
    font-size: 24rpx;
    line-height: 50rpx;
    text-align: center;
}
.sdr-box image{
    display: inline-block;
    width: 32rpx;
    height: 32rpx;
    vertical-align: middle;
}
/* tab列表 */
.tab{
    box-sizing: border-box;
    padding: 30rpx;
    width: 100%;
}
.tab-top{
    display:flex;
}
.tab-top view{
    flex: 1;
    text-align: center;
    font-size: 30rpx;
    color: #A8A8A8;
    line-height: 80rpx;
}
.tab-top view.on{
    color:red;
}
.tab-swiper{
    background:red;
}
.tab-swiper view{
    line-height: 200rpx;
    font-size: 40rpx;
    text-align: center;
}
/* 横向滚动tab */
.scroll-view {
    margin-top:10rpx;
    white-space: nowrap;
    overflow: hidden;
}
.sc-city{
    height: 80rpx;
    line-height: 80rpx;
    display: inline-block;
    padding: 0 30rpx;
    border: 1rpx solid #ccc;
    margin-right: 20rpx;
}
.sc-city.on{
    color:red
}
</style>