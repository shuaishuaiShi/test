<!-- 首页 -->
<template>
	<view class="main">
		<view class="content">
			<!-- <image class="logo" src="/static/logo.png"></image> -->
		</view>
		<view class="interactive">	
			<view class="touxiang">
				<image :src='user_tx' class="tx"></image>
			</view>
			<view class="w_tq">
				<view class="user_name">{{user_name}}-你好！</view>
				<text class="title">{{title}}</text>
			</view>
			<!-- 弹窗组件 -->
			<popup class="weather" :maskModelLogin="maskModelLogin" v-on:login="login" ref="popup"></popup>
			<!-- 天气组件 -->
			<view class="tq_data">
				<tqsj :city="city" :wendu="wendu" :fengli="fengli"></tqsj>
			</view>
			<view v-if="dl_ts" class="dl_ts">登录后可查看天气详情:</view>
			<view v-if="dl_ts">
				<pickerView class="picker" v-on:bindChange="bindChange"></pickerView>
			</view>
			<!-- <button @tap="denglu">登录</button> -->
			<button @tap="zhuxiao">注销</button>
			<view v-if="!dl_ts">
				<picker mode="region" :range="provinces" @change="bindChange1">
					<button>更换城市</button>
				</picker>
			</view>
			
		</view>
	</view>
</template>

<script>
	import popup from './Popup.vue'
	import tqsj from './muban.vue'
	import pickerView from './picker-view.vue'
	// import {App} from '../Api/xmadx_sdk.min.js'
	// import {sign_util} from '../Api/sign.js'
	
	export default {
		data() {
			return {
				title: '今天也是充满希望的一天',
				// 接收请求的天气信息
				json: {},
				// 接收组件信息
				city: '',
				wendu: '',
				fengli: '',
				// 用户登录请求的服务器地址
				url: 'https://www.kunyang0106.com/index.php/',
				// 提示登录的信息是否显示
				dl_ts: false,
				// 登录弹窗是否显示
				maskModelLogin: true,
				// 以下用来接收用户信息
				user_id: '',
				user_name: '',
				userCity: '',
				user_tx: '',
				// 地区选择器
				provinces:['更换城市'],
				provincesIndex: 0
			}
		},
		onLoad() {
			
		},
		onShow() {
			/*
			*获取用户登录态 登录 =》获取用户信息 =》请求用户所在城市的天气数据
			* 					  =》用户信息不包含城市 =》提示手动选择所在城市（暂无）
			*				 未登录 =》提示登录后可以获取准确天气信息 =》登录获取信息
			* 														=》拒绝登录 =》提示手动选择城市（暂无）
			*/
			var that = this;
			
			// 需要登录时使用
			// console.log(uni.getStorageSync('signIn_key'));
			// if(uni.getStorageSync('signIn_key')){
			// 	this.maskModelLogin = false;
			// 	this.dl_ts = false;
			// 	//获取用户信息（最新）
			// 	uni.getUserInfo({
			// 	  provider: 'weixin',
			// 	  success: function (infoRes) {
			// 		console.log(infoRes);
			// 		// 判断用户信息中是否包含城市
			// 		if(JSON.parse(infoRes.rawData).province){
			// 			// 接口需要小写的城市名称，所以将城市转为小写
			// 			that.userCity = JSON.parse(infoRes.rawData).province.toLowerCase();
			// 			// 用户现在所用头像
			// 			that.user_tx = JSON.parse(infoRes.rawData).avatarUrl;
			// 			// console.log(that.user_tx);
			// 			// 用户信息中的姓名
			// 			that.user_name = JSON.parse(infoRes.rawData).nickName;
			// 			// 请求用户所在城市的信息
			// 			that.request_weather(that.userCity);
			// 			// 弹窗不显示
			// 			that.maskModelLogin = false;
			// 		}else{
			// 			// 未获取到用户所在城市，需手动选择
			// 			console.log("未获取到用户所在城市");
			// 		}
			// 	  }
			// 	});
			// }else{
			// 	this.maskModelLogin = true;
			// 	this.dl_ts = true;
			// }
			
			// 模拟登录态登录
			if(uni.getStorageSync('moni_signIn_key')){
				// 	//获取用户信息（最新）
				uni.getUserInfo({
				  provider: 'weixin',
				  success: function (infoRes) {
					console.log(infoRes);
					// 判断用户信息中是否包含城市
					if(JSON.parse(infoRes.rawData).province){
						// 接口需要小写的城市名称，所以将城市转为小写
						that.userCity = JSON.parse(infoRes.rawData).province.toLowerCase();
						// 用户现在所用头像
						that.user_tx = JSON.parse(infoRes.rawData).avatarUrl;
						// console.log(that.user_tx);
						// 用户信息中的姓名
						that.user_name = JSON.parse(infoRes.rawData).nickName;
						// 请求用户所在城市的信息
						that.request_weather(that.userCity);
						// 弹窗不显示
						that.maskModelLogin = false;
						that.dl_ts = false;
					}else{
						// 未获取到用户所在城市，需手动选择
						console.log("未获取到用户所在城市");
					}
				}
			})
			}else{
				this.maskModelLogin = true;
				console.log("弹窗状态" + this.maskModelLogin);
				this.dl_ts = true;
				uni.getUserInfo({
					provider: 'weixin',
					success: function (infoRes) {
					console.log(infoRes);
					// 判断用户信息中是否包含城市
					// if(JSON.parse(infoRes.rawData).province){
						// 接口需要小写的城市名称，所以将城市转为小写
						that.userCity = JSON.parse(infoRes.rawData).province.toLowerCase();
						// 用户现在所用头像
						that.user_tx = JSON.parse(infoRes.rawData).avatarUrl;
						// console.log(that.user_tx);
						// 用户信息中的姓名
						that.user_name = JSON.parse(infoRes.rawData).nickName;
						// 请求用户所在城市的信息
						// that.request_weather(that.userCity);
						// 弹窗不显示
						// that.maskModelLogin = false;
					},
					fail: function(){
						uni.showLoading({
							title: '未获取你的信息'
						});
						setTimeout(function () {
							uni.hideLoading();
						}, 2000);
					}
				})
			}
		},
		onHide() {
			
		},
		components:{
			// 引用天气组件
			tqsj,
			// 引用弹窗组件
			popup,
			// 省份选择器组件
			pickerView
		},
		methods: {
			// 用户登录
			denglu: function(){
				var that = this;
				uni.login({
					// 向哪个平台请求
					provider: 'weixin',
					 success: res => {
						console.log(res.code);
						// 登录请求
						uni.request({
							url: that.url + 'v1/user',
							method: 'POST',
							data: {
								// 向服务器请求的参数
								jscode: res.code,
								// sign: "119b64d8cec3b85de15cf842ae5f9056",
								// time: '',
								// type: '',
								// user_id: ''
							},
							success: res => {
								// console.log("发送请求");
								console.log(res);
								console.log(res.data.data.userid);
								that.signKey(res.data.data.userid);
							},
							fail: () => {},
							complete: () => {}
						});
					},
					fail: () => {},
					complete: () => {}
				});
			},
			
			//弹窗按钮获取用户信息
			login: function(e){
				var that = this;
				// 把用户原始信息字符串转换成对象格式
				if(JSON.parse(e.detail.rawData).province){
					// 接口需要小写的城市名称
					that.userCity = JSON.parse(e.detail.rawData).province.toLowerCase();
					this.user_tx = JSON.parse(e.detail.rawData).avatarUrl;
					// console.log(this.user_tx);
					this.user_name = JSON.parse(e.detail.rawData).nickName;
					// console.log(this.user_name);
					// console.log(that.userCity);
					that.request_weather(that.userCity);
					this.maskModelLogin = false;
					this.dl_ts = false;
					// 模拟一个登录状态
					that.moni_signKey(1);
				}
				// 未获取到用户所在城市，需手动选择
				else{
					// 用户头像
					this.user_tx = JSON.parse(e.detail.rawData).avatarUrl;
					// 用户名称
					this.user_name = JSON.parse(e.detail.rawData).nickName;
					console.log("未获取到用户所在城市");
					this.maskModelLogin = false;
					uni.showLoading({
						title: '未获取所在城市'
					});
					setTimeout(function () {
						uni.hideLoading();
					}, 2000);
				}
			},
			// 请求获取天气 可以获取用户则自动请求用户所在城市的天气，不可获取用户信息则让用户选择所在城市
			request_weather: function(userCity){
				// console.log(userCity);
				uni.request({
					url: 'https://free-api.heweather.net/s6/weather/now?location='+ userCity +'&key=2776dacfb53f4c4c8d516cd1f3133e6b',
					method: 'GET',
					data: {},
					success: res => {
						var that = this;
						// console.log(res);
						// that.data = res.data;
						// console.log(that.data);
						that.city = res.data.HeWeather6[0].basic.parent_city;
						that.wendu = res.data.HeWeather6[0].now.fl;
						that.fengli = res.data.HeWeather6[0].now.wind_sc;
					},
					fail: () => {},
					complete: () => {}
				});
			},
			// 存储登录态数据
			signKey: function(e){
				try {
					uni.setStorageSync('signIn_key', e);
					console.log(e);
					console.log("登录态存储成功");
				} catch (e) {
					console.log("登录态存储失败");
				}
			},
			// 存储模拟登录态数据
			moni_signKey: function(e){
				try {
					uni.setStorageSync('moni_signIn_key', e);
					console.log(e);
					console.log("登录态存储成功");
				} catch (e) {
					console.log("登录态存储失败");
				}
			},
			// 用于页面显示时获取用户信息
			uGetuserinfo: function(){
				//获取用户信息（最新）
				uni.getUserInfo({
				  provider: 'weixin',
				  success: function (infoRes) {
					console.log(infoRes);
					that.json = JSON.parse(infoRes.rawData);
					console.log(that.json.province);
					// 判断用户信息中是否包含城市
					if(that.json.province){
						// 接口需要小写的城市名称，所以将城市转为小写
						that.userCity = that.json.province.toLowerCase();
						// 用户现在所用头像
						that.user_tx = that.json.avatarUrl;
						// console.log(that.user_tx);
						// 用户信息中的姓名
						that.user_name = that.json.nickName;
						// 请求用户所在城市的信息
						that.request_weather(that.userCity);
						// 弹窗不显示
						that.maskModelLogin = false;
					}else{
						// 未获取到用户所在城市，需手动选择
						console.log("未获取到用户所在城市");
					}
				  }
				});
			},
			// 取消登录态，用于测试
			zhuxiao: function(){
				uni.clearStorage();
				console.log("登录态注销成功");
				console.log(this.user_id);
			},
			// 调用 popup子组件的方法
			diaoyong: function(){
				var a = this.$refs.popup.login();
				console.log(a);
			},
			bindChange: function(e){
				var that = this;
				console.log(e);
				that.request_weather(e.detail.__args__[0]);
				that.dl_ts = false;
				console.log(this.dl_ts);
			},
			bindChange1: function(e){
				var that = this;
				console.log(e);
				that.request_weather(e.detail.code[2]);
			}
		}
	}
</script>

<style>
	.content {
		/* position: absolute; */
		/* width: 320px; */
		min-width: 320px;
		max-width: 1024px;
		/* height: 550px; */
		min-height: 568px;
		max-height: 1366px;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		background-image: url(https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3258691876,3535546281&fm=26&gp=0.jpg); opacity:0.5;
		background-size: 100% 100%;
		background-origin: 0.3;
	}
	
	.interactive{
		position: relative;
		margin-top: -150px;
		width: 200px;
	}
	
	.tx{
		position: absolute;
		width: 200upx;
		height: 200upx;
		margin-top: -300px;
		margin-left: 30%;
		margin-right: auto;
		margin-bottom: 50upx;
	}
	
	.tq_data{
		position: absolute;
		margin-top: -58%;
		width: 215px;
		text-align: center;
	}
	
	.user_name{
		position: absolute;
		margin-top: -108%;
		width: 215px;
		text-align: center;
	}
	
	.title{
		position: absolute;
		margin-top: -98%;
		width: 208px;
		text-align: center;
	}
	
	.dl_ts{
		text-align: center;
	}
	
	.logo{
		width: 200upx;
		height: 200upx;
		margin-top: 200upx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50upx;
	}
	
	.tx{
		width: 200upx;
		height: 200upx;
		border-radius: 50%;
	}
</style>
