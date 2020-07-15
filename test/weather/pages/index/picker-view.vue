<!-- 滚动选择器 -->
<template>
	<!-- <view>
		<view>{{province}}省{{city}}市</view>
		<picker mode="region" :range="selector" @change="bindChange">
			<picker-view indicator-style="height: 50px;" :value="value" @change="bindChange">
				<picker-view-column>
					<view v-for="(item,index) in provinces" style="line-height: 50px" :key="index">{{item}}省</view>
				</picker-view-column>
				<picker-view-column>
					<view v-for="(item,index) in citys" style="line-height: 50px" :key="index">{{item}}市</view>
				</picker-view-column>
			</picker-view>
		</picker>
	</view> -->
	<view v-if="dl_ts">
		<picker mode="region" :range="provinces" @change="bindChange">
			<view style="text-align: center;">未获取到您所在城市</view>
			<button>{{provinces[provincesIndex]}}</button>
		</picker>
	</view>
</template>

<script>
	export default{
		data() {
			return {
				provinces:['点击选择所在城市'],
				provincesIndex: 0,
				dl_ts: true
			}
		},
		methods:{
			request_weather: function(userCity){
				console.log("请求天气数据");
				console.log(userCity);
				uni.request({
					url: 'https://free-api.heweather.net/s6/weather/now?location='+ userCity +'&key=2776dacfb53f4c4c8d516cd1f3133e6b',
					method: 'GET',
					data: {},
					success: res => {
						var that = this;
						// console.log(res);
					},
					fail: () => {
						// console.log("天气数据请求失败");
					},
					complete: () => {
						// console.log("天气数据请求完毕");
					}
				});
			},
			bindChange: function(e){
				var that = this;
				console.log(e);
				// var sj = that.request_weather(e.detail.code[2]);
				this.$emit('bindChange',e.detail.code[2])
			}
		}
	}
</script>

<style>
</style>
