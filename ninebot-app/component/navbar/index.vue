<template>
		<scroll-view scroll-x="true" class="content"  :scroll-into-view="'item'+JSON.stringify(flag)" scroll-with-animation="true">
				<view class="content-item" v-for="item in info" @click="checkBar(item.id)" :id="'item'+item.id">
					<view class="icon" >
						<image class="unactive-ball" src="../../static/气球1.png" mode="widthFix" v-if="item.active"></image>
						<image class="unactive-ball" src="../../static/气球2.png" mode="widthFix" v-else></image>
						<image class="unactive-dot" src="../../static/圆点1.png" mode="widthFix" v-if="item.active"></image>
						<image class="unactive-dot" src="../../static/圆点2.png" mode="widthFix" v-else></image>
					</view>
					<view class="info" >
						<text class="info1" style="color: #C8C9CC;" v-if="item.active">{{item.text}}</text>
						<text class="info1" style="color: #808080;font-weight: 555;" v-else>{{item.text}}</text>
					</view>
				</view>
		</scroll-view>	
</template>

<script>
	export default {
		data(){
			return{
				temp:0,
			}
		},
		props:['info','flag'],
		created() {
			this.info[this.temp].active = false
		},
		methods: {
			checkBar(id){
				this.info[this.flag].active = true
				this.info[this.temp].active = true
				this.$emit('checkBar',id)
				this.info[id].active = false
				this.temp = id
			}
		}
	} 
</script>

<style lang="scss" scoped>

	.content{
		display: flex ;
		flex-direction: row;
	  white-space: nowrap;
		width: 100%;    
		.content-item{
			display: inline-flex;
			flex-direction: row;
			margin-left: 10rpx;
			.icon{
				display: flex;
				flex-direction: column;
				.unactive-ball{
					width: 20rpx;
					height: 30rpx;
					margin:16rpx auto 0 auto;
				}
				.unactive-dot{
					width: 3px;
					height: 3px;
					margin: 4rpx auto 0 auto;
				}
			}
			.info{
				display: flex;
				flex-direction: column;
				margin-left: 2rpx ;
				border-bottom: 4rpx dashed #C8C7CC;
				padding-right: 40rpx;
				.info1{
					padding-left: 4rpx;
					font-size: 14px;
					margin-top: 10rpx;
				}
			}
		}
	}



	
</style>
