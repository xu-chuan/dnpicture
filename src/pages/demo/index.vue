<template>
  <view @touchstart="handleTouchstart" @touchend="handleTouchend">
      学习触屏事件
  </view>
</template>

<script>
export default {
    data() {
        return {
            startTime: 0,
            startX: 0,
            startY: 0
        }
    },
methods: {
    handleTouchstart(event){
        // console.log("手指按下屏幕");
        // 加event参数是实现
        // console.log("按下:"+event.changedTouches[0].clientX);
        // console.log("按下:"+event.changedTouches[0].clientY);
        this.startTime = Date.now();
        this.startX = event.changedTouches[0].clientX;
        this.startY = event.changedTouches[0].clientY;
    },
    handleTouchend(event){
        // console.log("手指离开屏幕");
        // console.log("离开:"+event.changedTouches[0].clientX);
        // console.log("离开:"+event.changedTouches[0].clientY);
        const endTime = Date.now();
        const endX = event.changedTouches[0].clientX;
        const endY = event.changedTouches[0].clientY;

        // 判断按下的时长
        if(endTime-this.startTime>2000){
            return;
        }
        // 赋值滑动的方向"""
        let direction = "";
        
        // 先判断用户滑动的距离 是否合法 合法:判断滑动的方向 注意 距离要加绝对值
        if(Math.abs(endX-this.startX)>10){
            direction = endX - this.startX>0?"right":"left";
        }else{
            return;
        }
        // 用户做了合法的滑动操作
        console.log(direction);
        
    }
},
}
</script>

<style scoped>
view{
    width: 100%;
    height: 500rpx;
    background-color: aqua;
}
</style>