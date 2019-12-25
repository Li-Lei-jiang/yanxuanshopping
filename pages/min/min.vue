<template>
	<view>
		<view class="min_hed">
			<view>
				<view class="min_hed_tx" v-if="User.length<=0">
					<view class="min_hed_tp">
						<image src="../../static/icos/default-face.png"></image>
					</view>
					<view class="min_hed_txt" @click="tologin">登录/注册</view>
				</view>
				<view class="min_hed_tx" v-else>
					<view class="min_hed_tp">
						<image :src="User.faceImage||User.avatarUrl"></image>
					</view>
					<view class="User" >
						<view class="User_name">{{User.nickName}}</view>
						<!-- <view class="User_id">{{User.id}}</view> -->
						<view class="User_id">{{User.unionId}}</view>
					</view>
				</view>
			</view>
			<view class="min_hed_rig" @click="compileUser">
				<image src="../../static/icos/settings.png" mode=""></image>
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
				User:{},//用户对象
				openId:'',//令牌
				// userwx:{},//微信登录对象
				// userqq:{},//qq 登录对象
			}
		},
		methods: {
			//点击登录
			tologin() {
				uni.navigateTo({
					url: "login/login"
				})
			},
			getUser(){//获取用户信息
				this.User = uni.getStorageSync("userBO");
				this.openId = uni.getStorageSync("openId");				
				console.log(this.User)
				console.log(this.openId)
			},
			compileUser(){//去编辑页面
				uni.navigateTo({
					url:"/pages/min/compileUser/compileUser"
				})
			},
		},
		mounted() {

		},
		onLoad() {
			this.getUser()
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
	.min_hed {
		height: 300rpx;
		background: #FFD655;
		display: flex;
		justify-content: space-between;
	}

	.min_hed_tp {
		width: 180rpx;
		height: 140rpx;
	}

	.min_hed_tp image {
		width: 100%;
		height: 100%;
	}

	.min_hed_tx {
		display: flex;
		position: relative;
		bottom: -106rpx;
		height: 140rpx;
		left: 0;
		padding-left: 4%;
		line-height: 140rpx;

	}

	.min_hed_txt {
		padding-left: 3%;
		font-size: 20px;
		font-weight: bold;
		color: #6A5018;
		width: 100%;
	}
	.User{
		line-height: 64rpx;
		padding-left:5%;
	}
	.User_name{
		font-weight: bold;
	}
	.User_id{
		color: #C0C0C0;
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
	}
	.min_hed_rig{
		width: 50rpx;
		height: 50rpx;
		line-height: 300rpx;
		padding-right: 4%;
	}
	.min_hed_rig image{
		width: 100%;
		height:100%;
	}
</style>
