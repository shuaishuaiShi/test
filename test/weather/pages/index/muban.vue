<!-- 天气模板 -->
<template>
	<view class="main">
		<view class="top">
			<view class="city">你所在城市是: {{city}}</view>
			<view class="wendu">你所在城市的温度: {{wendu}}</view>
			<view class="fengli">你所在城市的风力等级: {{fengli}}</view>
		</view>
	</view>
</template>

<script>
	export default{
		// props用来组件传值
		props:{
			city:{
				type: String,
				default: '暂无数据'
			},
			wendu:{
				type: String,
				default: '暂无数据'
			},
			fengli:{
				type: String,
				default: '暂无数据'
			}
		},
		data(){
			return{
				
			}
		}
	}
</script>

<style>
	.main {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}
	.top{
		line-height: 25px;
	}
</style>
