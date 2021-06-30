<template>
	<view class="content">
		<image src="/static/index.png" mode="widthFix"></image>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello'
			}
		},
		onLoad() {
			setTimeout(()=>{
				uni.navigateTo({
					url: '../index/index'
				})
			},1000)
				
			
		},
		methods: {

		}
	}
</script>

<style>
	.content {
		width: 100%;
		height: 100%;
		display: flex;
		align-items: center;
		justify-content: center;		
	}
</style>
