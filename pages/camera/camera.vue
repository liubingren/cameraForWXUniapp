<template>
	<view :style="{width:width+'px', height:height+'px', overflow:'hidden'}">
		<camera device-position="back" flash="on" :style="{width:width+'px', height:height+'px',marginLeft:0+'px',position:'relative'}">
			<!-- 	<cover-image src='{{logo}}' class='takephoto' bindtap='takePhoto' style='width:150px; height:150px; position:absolute; bottom:30px; left:{{(width-150)/2}}px;'>
					</cover-image> -->
			
			<cover-view class='takephoto' @click='takePhoto' :style='{width:80+"px", height:80+"px", position:"absolute", bottom:30+"px",left:(width-80)/2+"px"}'></cover-view>
			<cover-view class="ps" :style='{left:(width-300)/2+"px"}'>请把创面图片置于绿框内</cover-view>
			<!--上部线条 -->
			<cover-view class='biaochi' :style='{top:0+"px", left:(width-375)/2+"px", width:375+"px", height:2+"px"}'>
			</cover-view>
			<!--下部线条 -->
			<cover-view class='biaochi' :style='{top:500+"px",left:(width-375)/2+"px", width:375+"px", height:2+"px"}'>
			</cover-view>
			<!--右部线条 -->
			<cover-view class='biaochi' :style='{top:0+"px", right:(width-375)/2+"px", width:2+"px",height:500+"px"}'> </cover-view>
			<!--左部线条 -->
			<cover-view class='biaochi' :style='{top:0+"px", left:(width-375)/2+"px", width:2+"px",height:500+"px"}'> </cover-view>
		</camera>
		
		
		
	</view>

</template>

<script>
	export default {
		data() {
			return {
				width: 0,
				height: 0,
				gap: 30

			}
		},

		onLoad() {
			let that = this
			// that.path = options.path
			that.ctx = wx.createCameraContext()

			wx.getSystemInfo({
				success: function(res) {
					that.width = res.screenWidth
					that.height = res.windowHeight
					// that.gap = 20
					console.log(res.screenWidth)
				}
			})
			
		},
		methods: {
			/**
			 * 拍照
			 */
			takePhoto() {
				console.log("MSBlobBuilder")
				var that = this
				that.ctx.takePhoto({
					quality: 'high',
					success: (res) => {
						console.log(res.tempImagePath)
						wx.navigateTo({
							url: '/pages/uploadImage/uploadImage?path=' + res.tempImagePath
						});
					}
				})
			}
		}
	}
</script>

<style lang="less" scoped>
	.takephoto {
		border-radius: 999px;
		box-shadow: 0 0 50rpx #ffd8a2;
		background-color: green;
	}
	.ps{
		color: #e23537;
		font-weight: bold;
		position: absolute;
		bottom: 250px;
		width: 300px;
		text-align: center;
		font-size: 18px;
	}
	.biaochi {
		background: green;
		position: absolute;
	}

	// .mask {
	// 	position: absolute;
	// 	widows: 100%;
	// 	height: 100%;
	// 	background-color: #808080;
	// 	top: 0;
	// 	left: 0;
	// }
</style>
