<template>
	<view>
		<canvas canvas-id="image-canvas" :style='{width:width+"px",height:500+"px"}'></canvas>
		<view class="info">
			<view class="info_item">
				<view></view>
				<view>病人基本信息</view>
			</view>
			<view class="info_item">
				<view>年龄</view>
				<view><input class="uni-input" v-model="age" type="number" placeholder="请输入你的年龄" /></view>
			</view>
			<view class="info_item">
				<view>性别</view>
				<view class="radio_group">
					<radio-group @change="radioChange">
						<label class="uni-list-cell uni-list-cell-pd" v-for="(item, index) in items" :key="item.value">
							<view>
								<radio :value="item.value" :checked="index === current" />
							</view>
							<view>{{item.name}}</view>
						</label>
					</radio-group>
				</view>
			</view>
		</view>
		<view class="btn-group">
			<button class="takeAgain" type="warn" @click="toCamera">重新拍照</button>
			<button class="confirm" type="primary" @click="goToFormpage">确&nbsp;认</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				width: 0,
				height: 0,
				gap: 30,
				tempFilePath: "",
				canvas: null,
				filePath: '',
				items: [{
						value: '女',
						name: '女',
						checked: 'true'
					},
					{
						value: 'M',
						name: '男'
					}
				],
				current: 0,
				age: 0
			}
		},
		/** 
		 * 生命周期函数--监听页面加载
		 */
		onLoad: function(options) {
			var that = this
			that.path = options.path

			wx.getImageInfo({
				src: that.path,
				success: function(res) {
					// _this.setData({
					// 	imgwidth: res.width,
					// 	imgheight: res.height,
					// })
					console.log("原图片宽高")
					console.log(res.width, res.height)
				}
			})
			wx.getSystemInfo({
				success: function(res) {
					that.width = res.screenWidth
					that.height = res.windowHeight
					// that.width = 375
					// that.height = 500
					// that.gap = 20
					console.log((that.width - 375) / 2)
					wx.getImageInfo({
						src: that.path,
						success: function(res) {
							that.canvas = wx.createCanvasContext("image-canvas", that)
							//过渡页面中，图片的路径坐标和大小
							that.canvas.drawImage(that.path, (that.width - 375) / 2, 0, that.width, that.height)
							// wx.showLoading({
							// 	title: '数据处理中',
							// 	mask: true
							// })

							that.canvas.setStrokeStyle('white')
							// 这里有一些很神奇的操作,总结就是MD拍出来的照片规格居然不是统一的
							//过渡页面中，对裁剪框的设定
							that.canvas.strokeRect((that.width - 375) / 2, 0, 375, 500)
							that.canvas.draw()

							setTimeout(function() {
								wx.canvasToTempFilePath({ //裁剪对参数
									canvasId: "image-canvas",
									x: (that.width - 375) / 2, //画布x轴起点
									y: 0, //画布y轴起点
									width: 375, //画布宽度
									height: 500, //画布高度
									destWidth: 375 * 4, //输出图片宽度
									destHeight: 500 * 4, //输出图片高度
									canvasId: 'image-canvas',
									success: function(res) {
										that.filePath = res.tempFilePath
										//清除画布上在该矩形区域内的内容。
										that.canvas.clearRect((that.width - 375) / 2, 0, 375, 500)
										// that.canvas.drawImage(that.filePath, that.gap, that.gap, that.width - that.gap *
										// 	2, that.width - that.gap * 2+40)
										// that.canvas.drawImage(that.filePath, 0, 0, that.width, that.height / 2 )
										that.canvas.drawImage(that.filePath, (that.width - 375) / 2, 0, 375, 500)
										that.canvas.draw()
										wx.hideLoading()
										//在此可进行网络请求

										wx.getImageInfo({
											src: res.tempFilePath,
											success: function(res) {
												// _this.setData({
												// 	imgwidth: res.width,
												// 	imgheight: res.height,
												// })
												console.log("裁剪后图片宽高")
												console.log(res.width, res.height)
											}
										})

									},
									fail: function(e) {
										wx.hideLoading()
										wx.showToast({
											title: '出错啦...',
											icon: 'loading'
										})
									}
								});
							}, 1000);
						}
					})
				}
			})
		},
		methods: {
			toCamera(){
				wx.switchTab({
				  url: '/pages/camera/camera'
				})
			},
			radioChange(evt) {
				for (let i = 0; i < this.items.length; i++) {
					if (this.items[i].value === evt.target.value) {
						this.current = i;
						break;
					}
				}
			},
			goToFormpage() {
				let info = {
					file: this.filePath,
					age: this.age * 1,
					sex: this.items[this.current]['value']
				}
				console.log(info)
				wx.navigateTo({
					url: '/pages/formPage/formPage?info=' + JSON.stringify(info)
				});
			}
		}
	}
</script>

<style scoped lang="less">
	.btn-group {
		margin-top: 5px;
		display: flex;
		flex-direction: row;

		.takeAgain {}

		.confirm {}
	}

	.info {
		background-color: #FFFFFF;
		margin: 40rpx 0;

		.info_item {
			display: flex;
			flex-direction: row;
			border-bottom: 1rpx solid #E5E5E5;
			height: 80rpx;
			line-height: 80rpx;

			&>view:nth-child(1) {
				width: 20vw;
				text-align: center;
				height: 80rpx;
				line-height: 80rpx;
				border-right: 1rpx solid #E5E5E5;
			}

			&>view:nth-child(2) {
				width: 79vw;
				height: 80rpx;
				margin-left: 25rpx;

				input {

					height: 80rpx;
					line-height: 80rpx;
				}
			}

			.radio_group {
				radio-group {
					display: flex;
					flex-direction: row;

					.uni-list-cell {
						display: flex;
						flex-direction: row;
						margin-left: 20rpx;
					}
				}

			}
		}
	}
</style>
