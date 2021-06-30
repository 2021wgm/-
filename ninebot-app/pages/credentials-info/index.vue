<!-- 证件上传信息页面 -->
<template>
	<view>
		<view class="content">
			证件上传信息页面
			<button @click="nextStep" class="btn">提交</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
	methods: {
		nextStep(){
			
		}
	},
	}
</script>

<style scoped>
	.content{
		width: 100%;
		height: 100%;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}
	.btn{
			width: 200rpx;
			height: 100rpx;
			background-color: #007AFF;
			top: 900rpx;
	}

</style>
