<template>
	<view>
		<view>
			<view class="loginhead">
				<view class="logintp">
					<image src="../../../static/icos/default-face.png" mode=""></image>
				</view>
			</view>
			<view>
				<view class="login_ip">
					<view class="login_iptx">账号</view>
					<view class="login_ip_input"><input type="text" value="" @input="inputVal" placeholder="请输入用户名" class="login_ip_inputs" /></view>
				</view>
				<view class="login_ip">
					<view class="login_iptx">密码</view>
					<view class="login_ip_input"><input type="password" @input="passwordVal" value="" placeholder="请输入密码" class="login_ip_inputs" /></view>
				</view>
			</view>
			<!-- #ifdef H5-->
			<view class="login_butt" @click="tologin">注册/登录</view>
			<!-- #endif -->

			<!-- #ifdef MP-WEIXIN -->
			<view class="login_butts">注册/登录</view>
			<view class="end">第三方账号登录</view>
			<view class="end_all">
				<view class="end_tp" @click="wxlogin">
					<image src="../../../static/icos/weixin.png" mode=""></image>
				</view>
				<!-- #endif -->

				<!-- #ifdef APP-PLUS -->
				<view class="login_butts">注册/登录</view>
				<view class="end">第三方账号登录</view>
				<view class="end_all">
					<view class="end_tp" @click="wxlogin">
						<image src="../../../static/icos/weixin.png" mode=""></image>
					</view>
					<view class="end_tp" @click="qqlogin">
						<image src="../../../static/icos/QQ.png" mode=""></image>
					</view>
					<view class="end_tp" @click="wblogin">
						<image src="../../../static/icos/weibo.png" mode=""></image>
					</view>
				</view>
				<!-- #endif -->
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: "",
		components: {

		},
		props: {},
		data() {
			return {
				username: '', //用户账号
				password: '', //,密码
				Openid: '', //令牌
			}
		},
		methods: {
			//点击登录 普通登录
			tologin() {
				console.log(this.username, this.password)
				uni.request({
					url: `${this.$api}/user/registOrLogin?qq=122212489`,
					method: 'POST',
					data: {
						userBO: {}, //用户对象	
						username: this.username, //用户账号
						password: this.password, //,密码
					},
					success: res => {
						if (res.statusCode === 200) {
							uni.showToast({
								title: '登录成功',
								duration: 2000
							})
							uni.switchTab({
								url: "/pages/min/min"
							})
							this.OpenId = res.data.data.userUniqueToken;
							uni.setStorageSync('OpenId', this.OpenId)
							uni.setStorageSync("userBO", res.data.data);
							
							// console.log(this.OpenId)
							// console.log(res.data.data.userUniqueToken)
							// console.log(uni.getStorageSync('userBO'))					
							// uni.setStorageSync("OpenId",this.OpenId)
						}
						// console.log(res)
					},
					fail: () => {},
					complete: () => {}
				});
				// 
			},
			inputVal(e) { //获取账号
				this.username = e.detail.value;
				// this.userBO.username = e.detail.value;
				// console.log(this.username)
			},
			passwordVal(e) { //获取密码
				this.password = e.detail.value;
				// this.userBO.password = e.detail.value;
				// console.log(this.password)
			},
			wxlogin() { //第三方微信登录
				uni.login({
					provider: 'weixin',
					success: (res) => {
						//获取用户信息
						uni.getUserInfo({
							provider: 'weixin',
							success: function(infoRes) {
								let openId = infoRes.userInfo.openId;
								uni.setStorageSync('openId');
								uni.setStorageSync('userBO',infoRes.userInfo);
								uni.showToast({
									title: '登录成功',
									duration: 2000
								})
								uni.switchTab({
									url: "/pages/min/min"
								})
								// console.log('用户昵称为：' + infoRes.userInfo.nickName);
								console.log(infoRes.userInfo)
							}
						})
						// console.log(res);
					}
				})
			},
			qqlogin() {
				uni.login({
					provider: 'qq',
					success: function(loginRes) {
						console.log(loginRes.authResult);
						uni.getUserInfo({
							provider: 'qq',
							success: function(infoRes) {
								let openId = infoRes.userInfo.openId;
								uni.setStorageSync('openId');
								uni.setStorageSync('userBO',infoRes.userInfo);
								uni.showToast({
									title: '登录成功',
									duration: 2000
								})
								uni.switchTab({
									url: "/pages/min/min"
								})
								// console.log('用户昵称为：' + infoRes.userInfo.nickName);
								console.log(infoRes.userInfo)
							}
						})
					}
				});
			},
			// wblogin(){
			// 	uni.login({
			// 	  provider: 'wb',
			// 	  success: function (loginRes) {
			// 	    console.log(loginRes.authResult);
			// 	  }
			// 	});
			// },
		},
		mounted() {

		},
		onLoad() {

		},
		filters: {

		},
		computed: {

		},
		watch: {

		},
		directives: {

		}
	}
</script>

<style scoped lang="scss">
	.loginhead {
		height: 400rpx;

	}

	.logintp {
		width: 150rpx;
		height: 150rpx;
		transform: translate(-50, -50);
		position: relative;
		top: 32%;
		left: 39%;
	}

	.logintp image {
		width: 100%;
		height: 100%;
	}

	.login_ip {
		display: flex;
		width: 95%;
		margin: 0 auto;
		border-bottom: 5rpx solid #F1F1F1;
		height: 100rpx;
		line-height: 100rpx;
	}

	.login_iptx {
		color: #CDCDCD;
		width: 196rpx;
		text-align: center;
	}

	.login_ip_input {
		width: 100%;
		height: 96rpx;
		line-height: 96rpx;
	}

	.login_ip_inputs {
		height: 98rpx;
		line-height: 98rpx;
	}

	/*#ifdef H5*/
	.login_butt {
		width: 67%;
		margin: 0 auto;
		height: 100ropx;
		line-height: 100rpx;
		color: white;
		text-align: center;
		background: #007AFF;
		margin-top: 10%;
	}

	/*#endif*/
	/*#ifdef MP-WEIXIN*/
	.login_butts {
		width: 67%;
		margin: 0 auto;
		height: 100ropx;
		line-height: 100rpx;
		color: white;
		text-align: center;
		background: #1AAD19;
		margin-top: 10%;
	}

	.end_tp {
		width: 70rpx;
		height: 70rpx;
		margin: 0 auto;
	}

	.end_tp image {
		width: 100%;
		height: 100%;
	}

	/*#endif*/
	
	/*#ifdef APP-PLUS*/
	.login_butts {
		width: 67%;
		margin: 0 auto;
		height: 100ropx;
		line-height: 100rpx;
		color: white;
		text-align: center;
		background: #1AAD19;
		margin-top: 10%;
	}
	.end {
		color: #C0C0C0;
		height: 100rpx;
		text-align: center;
		line-height: 100rpx;
		margin-top: 5%;
		font-size: 24rpx;
	}

	.end_tp {
		width: 70rpx;
		height: 70rpx;
		margin: 0 auto;
	}

	.end_tp image {
		width: 100%;
		height: 100%;
	}

	.end_all {
		display: flex;
		justify-content: space-around;
		width: 50%;
		margin: 0 auto;
	}

	/*#endif*/
	.end {
		color: #C0C0C0;
		height: 100rpx;
		text-align: center;
		line-height: 100rpx;
		margin-top: 5%;
		font-size: 24rpx;
	}

	.end_tp {
		width: 70rpx;
		height: 70rpx;
		margin: 0 auto;
	}

	.end_tp image {
		width: 100%;
		height: 100%;
	}

	.end_all {
		display: flex;
		justify-content: space-around;
		width: 50%;
		margin: 0 auto;
	}
</style>
