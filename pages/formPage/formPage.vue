<template>
	<view>
		<view class="uni-title uni-common-mt uni-common-pl">创面类型</view>
		<view class="uni-list">

			<radio-group @change="typeRadioChange">
				<label class="uni-list-cell uni-list-cell-pd" v-for="(item, index) in typeItems" :key="item.value">
					<view>
						<radio :value="item.value" :checked="index === typeCurrent" />
					</view>
					<view>{{item.name}}</view>
				</label>
			</radio-group>
			<view class="input_wrap">
				<view>输入类型</view>
				<view>
					<input type="text" v-model='type_ps' @focus="changTypeChecked" placeholder="请输入创面类型">
				</view>
			</view>
		</view>

		<view class="uni-title uni-common-mt uni-common-pl">创面成因</view>
		<view class="uni-list">
			<radio-group @change="reasonRadioChange">
				<label class="uni-list-cell uni-list-cell-pd" v-for="(item, index) in reasonItems" :key="item.value">
					<view>
						<radio :value="item.value" :checked="index === reasonCurrent" />
					</view>
					<view>{{item.name}}</view>
				</label>
			</radio-group>
			<view class="input_wrap">
				<view>输入成因</view>
				<view>
					<input type="text" v-model='reason_ps' @focus="changReasonChecked" placeholder="请输入输入成因">
				</view>
			</view>
		</view>

		<button class="confirm" type="primary" @click="submitTo">确&nbsp;认</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				typeItems: [{
						value: '皮肤坏死',
						name: '皮肤坏死',
						checked: 'true'
					},
					{
						value: '皮肤缺损',
						name: '皮肤缺损'
					},
					{
						value: '皮肤软组织感染',
						name: '皮肤软组织感染'
					},
					{
						value: '坏疽',
						name: '坏疽'
					},
					{
						value: '窦道',
						name: '窦道'
					},
					{
						value: '一度烧伤',
						name: '一度烧伤'
					},
					{
						value: '二度烧伤',
						name: '二度烧伤'
					},
					{
						value: '三度烧伤',
						name: '三度烧伤'
					},
					{
						value: '瘢痕愈合',
						name: '瘢痕愈合'
					},
					{
						value: '其他',
						name: '其他'
					}

				],
				typeCurrent: 0,
				info: null,
				type_ps: '',
				reasonItems: [{
					value: '糖尿病',
					name: '糖尿病',
					checked: 'true'
				}, {
					value: '血管疾病',
					name: '血管疾病'
				}, {
					value: '感染疾病',
					name: '感染疾病'
				}, {
					value: '压疮',
					name: '压疮'
				}, {
					value: '灼伤',
					name: '灼伤'
				}, {
					value: '热压伤',
					name: '热压伤'
				}, {
					value: '机械性损伤',
					name: '机械性损伤'
				}, {
					value: '医源性损伤',
					name: '医源性损伤'
				}, {
					value: '免疫疾病',
					name: '免疫疾病'
				}, {
					value: '痛风',
					name: '痛风'
				}, {
					value: '射线',
					name: '射线'
				}, {
					value: '药物',
					name: '药物'
				}, {
					value: '肿瘤',
					name: '肿瘤'
				}, {
					value: '其他',
					name: '其他'
				}],
				reasonCurrent: 0,
				reason_ps: '',
			}
		},
		onLoad(opt) {
			// console.log(JSON.parse(opt.info))
			this.info = JSON.parse(opt.info)
		},
		methods: {
			typeRadioChange(evt) {
				for (let i = 0; i < this.typeItems.length; i++) {
					if (this.typeItems[i].value === evt.target.value) {
						this.typeCurrent = i;
						break;
					}
				}
			},
			reasonRadioChange(evt) {
				for (let i = 0; i < this.reasonItems.length; i++) {
					if (this.reasonItems[i].value === evt.target.value) {
						this.reasonCurrent = i;
						break;
					}
				}
			},
			changTypeChecked() {
				this.typeCurrent = this.typeItems.length - 1
			},
			changReasonChecked() {
				this.reasonCurrent = this.reasonItems.length - 1
			},
			submitTo() {

				this.info['token'] = 'b94d27b9934d3e08a52e52d7da7dabfac484efe37a5380ee9088f7ace2efcde9'
				// this.info['type'] = this.typeItems[this.typeCurrent]['value']
				// this.info['type_ps'] = this.type_ps
				//this.info['cause'] = this.reasonItems[this.reasonCurrent]['value']
				// this.info['reason_ps'] = this.reason_ps

				if (this.typeCurrent == this.typeItems.length - 1) {
					this.info['type'] = this.type_ps
				} else {
					this.info['type'] = this.typeItems[this.typeCurrent]['value']
				}

				if (this.reasonCurrent == this.reasonItems.length - 1) {
					this.info['cause'] = this.reason_ps
				} else {
					this.info['cause'] = this.reasonItems[this.reasonCurrent]['value']
				}
				console.log(this.info)

				if (this.info['type'] == '') {
					uni.showToast({
						title: '创面类型不能为空!',
						icon: 'none',
						duration: 2000
					});
					return false;
				}
				
				if (this.info['cause'] == '') {
					uni.showToast({
						title: '创面成因不能为空!',
						icon: 'none',
						duration: 2000
					});
					return false;
				}
				
				wx.showLoading({
					title: '上传数据中，请稍等...',
				})
				
				wx.uploadFile({
					url: 'https://health.etalkai.com/skinphoto/annotation', //仅为示例，非真实的接口地址
					filePath: this.info.file,
					name: 'file',
					header: {
						"Content-Type": "multipart/form-data"
					},
					formData: {
						'token': this.info['token'],
						'age': this.info['age'],
						'sex': this.info['sex'],
						'type': this.info['type'],
						'cause': this.info['cause'],
					},
					success: function(res) {
						var data = res.data
						//do something
						console.log(res)
						if (res.statusCode == 200) {
							wx.hideLoading();
							wx.navigateTo({
								url: '/pages/status/status?status=success'
							});
						}else{
							wx.hideLoading();
							wx.navigateTo({
								url: '/pages/status/status?status=fail'
							});
						}
					},
					fail: function(res) {
						console.log("出错了:" + JSON.stringify(res));
					},
					complete: function() {
						// wx.hideLoading();
					}
				})


			}
		}
	}
</script>

<style scoped lang="less">
	.uni-title {
		height: 80rpx;
		line-height: 80rpx;
		text-align: center;
	}

	.uni-list-cell {
		display: flex;
		justify-content: flex-start;
		height: 80rpx;
		line-height: 80rpx;
		padding-left: 20rpx;
		border-top: 1rpx solid #E5E5E5;
	}

	.input_wrap {
		display: flex;
		flex-direction: row;
		height: 80rpx;
		line-height: 80rpx;
		padding-left: 20rpx;
		border-top: 1rpx solid #E5E5E5;
		border-bottom: 1rpx solid #E5E5E5;

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
	}

	button {
		margin: 20rpx auto;
		width: 40vw;
	}
</style>
