<template name="tempItem">
  <view v-if="maskModelLogin">
    <!-- 遮罩层 -->
    <view class="mask"></view>
    <!-- 弹窗 -->
    <view class='window'>
      <view class='window-eul'>微信授权</view>
      <view class='window-title'>
        <image mode='widthFix' src='https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=229448546,829752387&fm=26&gp=0.jpg'></image>
        申请获取以下权限：
      </view>
      <view class='window-line'></view>
      <view class='window-content'>
        <view class='window-circle'>
          <view></view>
        </view>
        <view class='window-item'>获得你的公开信息（昵称、头像等）</view>
      </view>
      <view class='window-bottom-btn'>
        <view>
          <button open-type="getUserInfo" @getuserinfo='login' hover-class="window-btn-hover-class" hover-start-time="0" hover-stay-time="0" class='window-btn'>确定</button>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
	export default{
		props:{
			maskModelLogin:{
				type: Boolean,
				default: false
			}
		},
		data(){
			return {
				// maskModelLogin: false
			}
		},
		onLoad() {
			console.log("子组件：" + props.maskModelLogin.Boolean);
		},
		components:{
			
		},
		methods:{
			login: function(e){
				// console.log(e);
				this.$emit('login',e);
			}
		}
	}
</script>

<style>
.mask {
  width: 1024px;
  height: 1000px;
  background-color: skyblue;
  opacity: 0.5;
  position: fixed;
  z-index: 200;
  margin-top: -150% ;
  margin-left: -50%;
}

.window {
  height: 520rpx;
  width: 570rpx;
  background-color: #F0AD4E;
  position: fixed;
  z-index: 201;
  top: 50%;
  margin-top: -260rpx;
  left: 50%;
  margin-left: -285rpx;
  border-radius: 10rpx;
  /* display: none; */
}

.window-eul {
  font-size: 40rpx;
  margin-left: 50rpx;
  margin-top: 50rpx;
}

.window-title {
  font-size: 30rpx;
  margin-left: 50rpx;
  margin-top: 39rpx;
  display: flex;
  align-items: center;
}

.window-title image {
  width: 100rpx;
  border-radius: 50%;
  margin-right: 21rpx;
}

.window-line {
  height: 1px;
  width: 470rpx;
  margin-left: 50rpx;
  background-color: #dcdcdc;
  margin-top: 30rpx;
}

.window-content {
  margin-top: 40rpx;
  margin-left: 50rpx;
  margin-right: 50rpx;
  display: flex;
  align-items: center;
}

.window-circle {
  height: 25rpx;
  width: 25rpx;
}

.window-circle view {
  height: 20rpx;
  width: 20rpx;
  background-color: #999;
  border-radius: 50%;
}

.window-item {
  margin-left: 25rpx;
  font-size: 32rpx;
  color: #666;
  font-weight: 300;
}

.window-bottom-btn {
  height: 80rpx;
  width: 460rpx;
  margin-left: 50rpx;
  display: flex;
  align-items: center;
  justify-content: flex-end;
}

.window-bottom-btn view {
  width: 120rpx;
  height: 60rpx;
}

.window-btn::after {
  border: none;
  border-radius: 1rpx;
  margin: 0;
}

.window-btn {
  width: 100rpx;
  height: 70rpx;
  background: none;
  border: none;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 28rpx;
  padding: 0;
  color: #2f9e2d;
  border-radius: 2rpx;
}

.window-btn-hover-class {
  width: 100rpx;
  height: 70rpx;
  background: none;
  display: flex;
  border-radius: 2rpx;
  align-items: center;
  justify-content: center;
  border: none;
  font-size: 28rpx;
  padding: 0;
  color: #2f9e2d;
  background-color: #dcdcdc;
}
</style>
