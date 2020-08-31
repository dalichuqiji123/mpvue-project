<template>
  <div class="home">
      <!-- 轮播图 -->
    <swiper 
        :indicator-dots="true"
        :circular="true"
        :autoplay="true" 
        :interval="2000" 
        :duration="500"
        class='hs-swiper'
        >
        <block v-for="item in imgArr" :key="item.id">
          <swiper-item >
            <img :src="item.src" class="hs-img"/>
          </swiper-item>
        </block>
    </swiper>
    <!-- 搜索框 -->
    <view class="hs-search">
        <input class="hs-input" disabled placeholder="手镯"/>
        <img :src="img.searchIcon" class="hs-icon" >
    </view>
    <!-- 下拉列表 -->
    <view class="hs-picker">
        <picker 
        @change="catesChange" 
        :value="cateIndex" 
        :range="cates"
        range-key='cate_ch'
        class="hs-pickerLeft"
    >
        <view class="hsp-view">
            <text v-text='cates[cateIndex].cate_ch'></text>
            <image :src="img.filterIcon" class="hspv-icon" />
        </view>
    </picker>
    <picker 
        mode="region" 
        @change="cityChange" 
        :value="region" 
        :custom-item="customItem"
        class="hs-pickerRight"
    >
        <view class="hsp-view">
            当前区域：{{region[2]}}
            <image :src="img.filterIcon" class="hspv-icon" />
        </view>
    </picker>
    </view>
    <!-- 商品列表 -->
    <view class="home-list">
        <good />
    </view>
  </div>
</template>

<script>
import good from '@/components/good.vue'
import img from '@/utils/img'
export default {
    components:{
      good
    },
  data:function(){
      return {
          imgArr:[
              {id:1,src:'//m.360buyimg.com/mobilecms/s700x280_jfs/t1/146841/16/5445/146609/5f360511E77af7a5f/5ef2931e43cccca0.jpg!cr_1125x445_0_171!q70.jpg.dpg'},
              {id:2,src:'//m.360buyimg.com/mobilecms/s700x280_jfs/t1/131135/3/8110/45108/5f43e886Edb6dc5a1/1f9898fe716fa8e0.jpg!cr_1125x445_0_171!q70.jpg.dpg'},
              {id:3,src:'//m.360buyimg.com/mobilecms/s700x280_jfs/t1/115335/11/16175/151316/5f44d752E9dc49974/8524c1c5aa582c69.jpg!cr_1125x445_0_171!q70.jpg.dpg'},
              {id:4,src:'//m.360buyimg.com/mobilecms/s700x280_jfs/t1/148645/8/6360/139010/5f41f2a8Ede064732/68ffc70d87347782.jpg!cr_1125x445_0_171!q70.jpg.dpg'},
          ],
          img,
          cates:[
              {id:1,cate:"",cate_ch:'全部品类'},
              {id:2,cate:"sports" ,cate_ch:"体育用品"},
              {id:3,cate:"clother" ,cate_ch:"服装首饰"},
              {id:4,cate:"mother" ,cate_ch:"母婴用品"},
          ],
          region:["广东省","深圳市","宝安区"],
          customItem:'全部',
          cateIndex:'0'
      }
  },
  methods:{
      catesChange(e){
          this.cateIndex=e.target.value
      },
      cityChange(e){
          console.log(e)
          this.region=e.target.value
      }
  }
}
</script>

<style>
/* 轮播图 */
.hs-swiper{
    width:100%;
    height:528rpx;
}
.hs-img{
    display:block;
    width:100%;
    height:100%;
}
/* 搜索框 */
.hs-search{
    box-sizing: border-box;
    padding:14rpx 30rpx;
    position:relative;
    background:white;

}
.hs-input{
    width:690rpx;
    height:60rpx;
    margin:0 auto;
    padding-left:30px;
    background:rgba(239,239,239,1);
    border-radius: 30rpx;
    box-sizing: border-box;
    font-size:24rpx;
}
.hs-icon{
    position: absolute;
  left: 49rpx;
  top: 30rpx;
  width: 30rpx;
  height: 30rpx;
}
/* 筛选框 */
.hs-picker{
    display:flex;
    width:100%;
    height:80rpx;
    line-height: 80rpx;
}
.hs-pickerLeft{
    flex:1;
}
.hs-pickerRight{
    flex:1;
}
.hsp-view{
    margin:0 auto;
    text-align: center;
    height:80rpx;
    line-height: 80rpx;
}
.hspv-icon{
    width:28rpx;
    height:28rpx;
    vertical-align: middle;
}
.home-list {
  padding-top: 20rpx;
  background:rgba(255,255,255,1);
}
</style>
