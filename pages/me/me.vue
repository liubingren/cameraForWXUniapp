<template>
	<view class="me_wrap">
		<view class="me_top">
			<image class="avatarImg" :src="userinfo.avatarUrl" mode=""></image>
			<text>{{userinfo.nickName}}</text>
		</view>

		<button open-type="getPhoneNumber" @getphonenumber="getPhoneNumber"  style="width: 24vw;" v-if="isLogin !== true">登录</button>
		<button @click="logOut" style="width: 24vw;" v-else>退出</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				isLogin: false,
				userinfo: {
					avatarUrl: '/static/nologin.png',
					nickName:''
				}
			};
		},

		methods: {
			logOut() {
				this.isLogin = false
				this.userinfo = {
					avatarUrl: '/static/nologin.png'
				}
				wx.removeStorage({
					key: 'userinfo',
					success(res) {}
				})
			},
			getPhoneNumber(e) {
				let vm = this
				wx.login({
					success: resp => {
						// 发送 res.code 到后台换取 openId, sessionKey, unionId
						// var that = this;
						// that.codeStr = resp.code
						// 获取用户信息
						console.log('login')
						console.log(resp)
						wx.getSetting({
							success: res => {
								console.log(res)
								if (res.authSetting['scope.userInfo']) { 
									wx.getUserInfo({
										success: userResult => {
											var platUserInfoMap = {}
											platUserInfoMap["encryptedData"] = e.detail.encryptedData;
											platUserInfoMap["iv"] = e.detail.iv;
											let data = { 
												platCode: resp.code,
												platUserInfoMap: platUserInfoMap,
											}
											console.log(data)
											uni.request({
												url: 'https://health.etalkai.com/skinphoto/wechatapp/login',
												method: 'POST',
												data: data,
												success: res => {
													//vm.phonenumber = JSON.parse(res.data).userinfo.phoneNumber
													// console.log(JSON.parse(res.data))
													console.log(res)
												},
												fail: () => {},
												complete: () => {}
											});
										}
									})
								}
							}
						})
					}
				})
			},
		}
	};
</script>

<style lang="less" scoped>
	.me_wrap {
		height: 100vh;

		.me_top {
			width: 100%;
			height: 30vh;
			background-color: #c31323;
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;

			.avatarImg {
				height: 25vw;
				width: 25vw;
				border-radius: 50%;

			}
		}
		
		text {
			margin: 2vh 0;
			color: #FFFFFF;
		}
		
		button {
			margin-top: 2vh;
			color: #000000;
		}
	}
</style>
