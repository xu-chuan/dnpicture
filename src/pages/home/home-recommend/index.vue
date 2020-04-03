<template>
  <scroll-view @scrolltolower="handleToLower" class="recommends_view" scroll-y v-if="recommends.length>0">
    <!-- 推荐 开始 -->
    <view class="recommend_wrap">
      <navigator 
        class="recommend_item"
        v-for="item in recommends"
        :key="item.id"
        :url="`/pages/album/index?id=${item.target}`"
      >
        <image :src="item.thumb" mode="widthFix"></image>

      </navigator>
    </view>
    <!-- 月份 开始 -->
    <view class="months_wrap">
       <view class="months_title">
         <view class="months_title_info">
           <view class="months_info">
             <text> {{months.DD}} / </text>
             {{months.MM}} 月
           </view>
           <view class="months_text">{{months.title}}</view>
         </view>
         <view class="months_title_more">更多 > </view>
       </view>
       <view class="months_content">
         <view class="months_item"
         v-for="item in months.items"
         :key="item.id"
         >
         <image :src="item.thumb+item.rule.replace('$<Height>',360)" mode="aspectFill"></image>
         </view> 
       </view>
    </view>
    <!-- 热门 开始 -->
    <view class="hots_wrap">
      <view class="hots_tilte"> 
        <text> 热门 </text>
        </view>
      <view class="hots_content">
        <view class="hot_item"
        v-for="item in hots"
        :key="item.id"
        >
        <image :src="item.thumb" mode="widthFix"></image>
        </view>
      </view>
    </view>
  </scroll-view>
</template>

<script>
import moment from "moment";
export default {

  data(){
    return{
      recommends:[],
      months:{},
      hots: [],
      // 请求的参数
      params: {
        limit: 30,
        order: "hot",
        skip: 0
      },
      // 是否还有下一页
      hasMore: true
    }
  },
  mounted() {
    this.getList();
  },
  methods: {
    // 获取接口的数据
    getList(){
      this.request({
      url: "http://157.122.54.189:9088/image/v3/homepage/vertical",
      data: this.params,
    }).then(result => {
      // console.log(result);
      
      // 判断还有没有下一页数据
      if (result.res.vertical.length === 0){
        this.hasMore=false;
        uni.showToast({
          title:"没有更多数据了",
          icon: "none"
        });
        return;
      }
      
      if (this.recommends.length === 0) {
        this.recommends = result.res.homepage[1].items;
        this.months = result.res.homepage[2];
        // 将时间戳改成 18号/月 moment.js
        this.months.MM=moment(this.months.stime).format("MM");
        this.months.DD=moment(this.months.stime).format("DD");
        // console.log(this.months);
      }
      
      // 获取热门数据的列表
      // 数组拼接 es6
      this.hots = [...this.hots, ...result.res.vertical];
    });
    },
    // 滚动调触底
    handleToLower() {
      // console.log("到底了");
      if(this.hasMore){
        this.params.skip += this.params.limit;
      this.getList();
      }else{
        // 弹窗提示
        uni.showToast({
          title:"没有数据了",
          icon:"none"
        })
      }
    }
  },
};
</script>
  
<style lang="scss" scoped>
.recommends_view{
  // height: 屏幕的高度 - 头部标题的高度
  height: calc( 100vh - 36px );
}

  .recommend_wrap{
    display: flex;
    flex-wrap: wrap; 
    .recommend_item{
      width: 50%;
      border: 5rpx soild #fff;
    }
  }
  .months_wrap {
  .months_title {
    display: flex;
    justify-content: space-between;
    padding: 20rpx;
    .months_title_info {
      color: $color;
      font-size: 30rpx;
      font-weight: 600;
      display: flex;
      .months_info {
        text {
          font-size: 36rpx;
        }
      }
      .months_text {
        font-size: 34rpx;
          color: #666;
          margin-left: 30rpx;
      }
    }

    .months_title_more {
      font-size: 24rpx;
      color: $color;
    }
  }

  .months_content {
      display: flex;
      flex-wrap: wrap;
      .months_item{
        width: 50%;
        border: 5rpx soild #FFF;
      }
  }
}
.hots_wrap {
    .hots_tilte {
      padding: 20rpx;
      text {
        border-left: 20rpx solid $color;
        padding-left: 20rpx;
        font-size: 34rpx;
        font-weight: 600;
      }
    }

    .hots_content {
      display: flex;
      flex-wrap: wrap;
      .hot_item {
        width: 33.33%;
        border: 5rpx solid #fff;
      
    }
  }
}
</style>