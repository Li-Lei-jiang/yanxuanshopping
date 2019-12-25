<template>
	<view>
		<view>
			<view class="compile">
				<view>头像</view>
				<view class="L0">
					<view class="compile_tp" v-if="this.file.length<=0"><image :src="User.faceImage" mode=""></image></view>
					<view class="compile_tp" v-else><image :src="this.file" mode=""></image></view>
					<view @click="toamend">></view>
				</view>
			</view>
			<view class="compile">
				<view>昵称</view>
				<view>{{User.nickname}} ></view>
			</view>
			<view class="compile">
				<view>生日</view>
				<view>></view>
			</view>
			<view class="compile">
				<view>性别</view>
				<view>></view>
			</view>
		</view>
		<uni-popup ref="popup" type="bottom" >
			<view class="popup" @click="Looks">查看我的头像</view>
			<view class="popup" @click="imagelideshow">从手机相册中选中择</view>
			<view class="popup" @click="close">取消</view>
		</uni-popup>
	</view>
</template>

<script>
	import uniPopup from "../../../components/uni-ui/uni-popup/uni-popup.vue"
	export default {
		name: "",
		components: {
			uniPopup
		},
		props: {},
		data() {
			return {
				User:{},//用户信息
				file:'',//图片
			}
		},
		methods: {
			getUser(){//获取用户信息
				this.User = uni.getStorageSync("userBO")
				console.log(this.User)
			},
			//弹出修改页
			toamend(){
				// 需要在 popup 组件，指定 ref 为 popup
				this.$refs.popup.open()
				// console.log(1)
			},
			close(){//取消弹窗
			this.$refs.popup.close()				
			},
			//图片预览
			Looks(){
				// uni.chooseImage({
				// 	count:1,
				// 	sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
				// 	success: function (res){
				// 		//
				// 	}
				// })
			},
			//选择相册图片
			imagelideshow(){
				let userId = this.User.id
				// console.log(this.User.id)
				uni.chooseImage({			
				    count: 1, //默认9
				    sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
				    sourceType: ['album'], //从相册选择album 参数
				    success: function (res) {
						this.file = res.tempFilePaths[0];
						// this.close()						// console.log(res.tempFilePaths[0]);
						// let file = JSON.stringify((res.tempFilePaths[0]));	
						// uni.request({							
						// 	url: `${this.$api}/user/uploadFace?qq=122212489`,
						// 	method: 'POST',
						// 	data: {
						// 	userId:userId,
						// 	file:file,
						// 	},
						// 	success: res => {
						// 		console.log(res)
						// 	},
						// 	fail: () => {},
						// 	complete: () => {}
						// });
						
						// this.faceImage = JSON.stringify(res.tempFilePaths)
				        // console.log(JSON.stringify(res.tempFilePaths));
				    }
				});
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
.compile{
	display: flex;
	justify-content: space-between;
	height:100rpx;
	line-height: 100rpx;
	border-bottom: 1rpx solid #CCCCCC;
	padding:0 3%;
}
.compile_tp{
	height: 100rpx;
	width: 100rpx;
	
}
.compile_tp image{
	height: 100%;
	width: 100%;
}
.L0{
	display: flex;
}
.popup{
	height: 100rpx;
	line-height: 100rpx;
	background: white;
	text-align:center;
	border-bottom: 1rpx solid #C0C0C0;
}
</style>
