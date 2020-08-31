<template>
  <div>
    <picker 
      mode="multiSelector" 
      @change="cityChange" 
      :value="listArr" 
      :range="arr"
      range-key="city"
      @columnchange='colChange'
      class='cityPicker'
    >
    <view class="picker">
      <text v-text="arr[0][listArr[0]].city"></text>
      <text>----</text>
      <text v-text="arr[1][listArr[1]].city"></text>
      <text>----</text>
      <text v-text="arr[2][listArr[2]].city"></text>
    </view>
  </picker>
  </div>
</template>

<script>
export default {
  data:function(){
    return {
      listArr:[0,0,0],
      arr:[],
      list:[
        [
          {id:1,city:"江西省"},
          {id:2,city:"广东省"},
          {id:3,city:"海南省"},
        ],
        [
          {id:11,pid:1,city:"南昌市"},
          {id:12,pid:1,city:"赣州市"},
          {id:21,pid:2,city:"深圳市"},
          {id:22,pid:2,city:"广州市"},
          {id:31,pid:3,city:"文昌市"},
        ],
        [
          {id:111,pid:11,city:"进贤县"},
          {id:121,pid:12,city:"九江县"},
          {id:112,pid:11,city:"南昌县"},
          {id:122,pid:12,city:"赣州县"},
          {id:211,pid:21,city:"宝安县"},
          {id:221,pid:22,city:"广州县"},
          {id:311,pid:31,city:"文昌县"}
        ]
      ]
    }
  },
  //初始化页面
  onShow(){
    this.filterCity(0,0)
  },
  methods:{
    // 如果不写这个的话那么最后一个就选择不了了，因为在封装的函数中listArr的二号位索引是0
    cityChange(e){
      this.listArr=e.target.value
    },
    // 调用封装函数filterCity这个函数
    colChange(e){
      this.filterCity(parseInt(e.target.column),parseInt(e.target.value))
    },
    // 封装一个筛选的函数，能根据前面父级选择的值确定后面显示的值
    filterCity(col,row){
      // 定义一个新的数组，这个数组就相当于后期筛选之后的数组，然后在picker中显示这个数组的内容
      let arr=this.arr
      let list =this.list
      arr[0]=list[0]
      // 当改变的是第一列的值时
      if(col==0){
        // 新数组arr的第二列就是根据新数组arr的第一列的东西来筛选的（更新第二列数据）
        arr[1]=list[1].filter(ele=>ele.pid==arr[0][row].id)
        // 更新第三列数据
        arr[2]=list[2].filter(ele=>ele.pid==arr[1][0].id)
        this.listArr=[row,0,0]
      }
      // 当改变的是第二列数据时
      if(col==1){
        // 只更新第三列
        arr[2]=list[2].filter(ele=>ele.pid==arr[1][row].id)
        this.listArr=[this.listArr[0],row,0]
      }
      // 更新arr
      this.arr=arr
    }
  }
}
</script>

<style>
.cityPicker{
  box-sizing: border-box;
  padding-top:200rpx;
}
</style>
