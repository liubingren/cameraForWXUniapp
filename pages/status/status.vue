<template>
	<view class="status_wrap">
		<view class="content" v-if="status == 'success'">
			<icon class="icon-box-img" type="success" size="70"></icon>
			<view>上传成功</view>
			<view>图片上传成功,继续拍照或退出</view>
			<button @click="toCamera" type="primary">继续拍照</button>
		</view>
		<view class="content" v-else>
			<icon class="icon-box-img" type="warn" size="70"></icon>
			<view>上传失败</view>
			<view>图片上传失败,继续拍照或退出</view>
			<button @click="toCamera" type="primary">继续拍照</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				status: ''
			};
		},
		onLoad(opt) {
			console.log(opt)
			this.status = opt.status

		},
		methods: {
			toCamera(){
				wx.switchTab({
				  url: '/pages/camera/camera'
				})
			}
		}
	}
</script>

<style lang="less" scoped>
	.status_wrap {
		width: 100vw;
		height: 100vh;
		.content{
			width: 100%;
			height: 100%;
			display: flex;
			flex-direction: column;
			// justify-content: center;
			align-items: center;
			icon{
				margin-top: 10vh;
				margin-bottom: 2vh;
			}
			button{
				margin-top: 2vh;
				width: 70vw;
			}
			view:nth-of-type(1){
				font-size: 40rpx;
				margin-bottom: 2vh;
			}
			view:nth-of-type(2){
				font-size: 24rpx;
			}
		}
	}
</style>
