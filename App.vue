<script>
	export default {
		data() {
			return {
				loginDataKey: 'loginData'
			};
		},
		onLaunch: function() {
			// let _this = this
			// const loginData = wx.getStorageSync(_this.loginDataKey)
			// let toLogin = () => {
			// 	console.log('未登录，去登录..')
			// 	wx.showLoading({
			// 		title: '登录中',
			// 		icon: 'loading',
			// 		mask: true
			// 	})
			// 	wx.login({
			// 		success(res) {
			// 			console.log('wx.login res')
			// 			console.log(res) 
			// 			let success = () => {
			// 				console.log('登录成功')
			// 				let loginResponse = {
			// 					"openid": "ooZAbwpdDdaZV_xiamianshi_mashaike"
			// 				}
			// 				wx.setStorageSync(_this.loginDataKey, loginResponse)
			// 				wx.hideLoading()
			// 			}
			// 			let fail = () => {
			// 				wx.hideLoading()
			// 				wx.showToast({
			// 					title: '登录失败，请重新打开小程序试试',
			// 					icon: 'none',
			// 					duration: 1e8
			// 				})
			// 			}
			// 			// TODO 发起网络请求
			// 			// 登录成功
			// 			success()
			// 		},
			// 		fail(err) {
			// 			console.log(err)
			// 			wx.hideLoading()
			// 			wx.showToast({
			// 				title: '登录失败，请重新打开小程序试试',
			// 				icon: 'none',
			// 				duration: 1e8
			// 			})
			// 		}
			// 	})
			// }
			// console.log('--loginData--')
			// console.log(loginData)
			// // 未登录，去登录
			// if (!loginData) {
			// 	toLogin()
			// } else {
			// 	wx.checkSession({
			// 		success() {
			// 			//session_key 未过期，并且在本生命周期一直有效
			// 			console.log('session_key 未过期')
			// 		},
			// 		fail() {
			// 			// session_key 已经失效，需要重新执行登录流程
			// 			// wx.login() //重新登录
			// 			console.log('session_key 已经失效，需要重新执行登录流程, 重新登录中')
			// 			toLogin()
			// 		}
			// 	})
			// }


		},
		onShow: function() {
			console.log('App Show');
		},
		onHide: function() {
			console.log('App Hide');
		}
	};
</script>

<style>
	/* 解决头条小程序组件内引入字体不生效的问题 */
	/* #ifdef MP-TOUTIAO */
	@font-face {
		font-family: uniicons;
		src: url('/static/uni.ttf');
	}

	/* #endif */
	page {
		background-color: #f4f4f4;
	}
</style>
