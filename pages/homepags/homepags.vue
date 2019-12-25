<template>
	<view>
		<!-- 轮播图 -->
		<view>
			<swiper :indicator-dots="true" :autoplay="true" :interval="5000" :duration="1000" style="height:400rpx;"
			 indicator-color="black" circular="true" indicator-active-color="white">
				<swiper-item v-for="(item,index) in slideshow" :key="index">
					<view class="slideshow_tp">
						<image :src="item.image"></image>
					</view>
				</swiper-item>
			</swiper>
		</view>
		<!-- 热门超英 -->
		<view class="hot_">
			<view class="hot_tp">
				<image src="../../static/icos/hot.png"></image>
			</view>
			<view class="hot_title">热门超英</view>
		</view>
		<view>
			<scroll-view scroll-x="true" style="width: 100%;">
				<view class="preview">
					<view v-for="(item,index) in HomeData" :key="index" class="preview_item">
						<view class="preview_tp">
							<image :src="item.cover" mode=""></image>
						</view>
						<view class="preview_name">{{item.name}}</view>
						<view class="preview_xing">
							<view class="preview_xing_x">
								<uni-rate disabled="true" :value="item.score/2" size="22" color="#ececec"></uni-rate>
							</view>
							<view>{{item.score}}</view>
						</view>
					</view>
				</view>
			</scroll-view>
		</view>
		<!-- 热门预告 -->
		<view class="hot_">
			<view class="hot_tp">
				<image src="../../static/icos/interest.png"></image>
			</view>
			<view class="hot_title">热门预告</view>
		</view>
		<view class="HomeData_vide">
			<view v-for="(item,index) in trailer" :key="index" class="vide_k">
				<video :src="item.trailer"></video>
			</view>
		</view>
		<!-- 猜你喜欢 -->
		<view style="margin-top: 10%;">
			<view  v-for="(item,index) in love" :key="index" class="love_item" >
				<view class="love_item_all">
					<view class="love_item_all_tp">
						<image :src="item.cover" mode=""></image>
					</view>
					<view class="love_item_center">
						<view class="love_item_center_name">
							{{item.name}}
						</view>
						<view class="love_xing">
							<view class="love_xing_">
								<uni-rate disabled="true" :value="item.score/2" size="22" color="#ececec"></uni-rate>
							</view>
						</view>
						<view class="love_item_basicInfo">{{item.basicInfo}}</view>
						<view class="love_item_basicInfo love_item_releaseDate">{{item.releaseDate}}</view>
					</view>
				</view>
				<br>
				<view class="love_rig">
					<view class="zan" @click="toZan">
						<image src="../../static/icos/praise.png" mode=""></image>
					</view>
					<view class="love_rig_txt">赞一下</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	import uniRate from '../../components/uni-ui/uni-rate/uni-rate.vue'
	export default {
		name: "",
		components: {
			uniRate
		},
		props: {},
		data() {
			return {
				slideshow: [], //轮播图
				HomeData: [], //首页数据
				trailer: [], //预告
				love: [], //猜你喜欢
			}
		},
		methods: {
			//获取轮播图
			getslideshow() {
				uni.request({
					url: `${this.$api}/index/carousel/list?qq=122212489`,
					method: 'POST',
					data: {},
					success: res => {
						this.slideshow = res.data.data;
						// console.log(this.slideshow)
						// console.log(res)
						this.getHomeData()
					},
					fail: () => {},
					complete: () => {}
				});
			},
			//获取首页数据
			getHomeData() {
				uni.request({
					url: `${this.$api}/index/movie/hot?qq=122212489&type=superhero`,
					method: 'POST',
					data: {},
					success: res => {
						this.HomeData = res.data.data;
						// console.log(this.HomeData)
						this.getHomeDataPrevue()
					},
					fail: () => {},
					complete: () => {}
				});
			},
			//获取热门预预告
			getHomeDataPrevue() {
				uni.request({
					url: `${this.$api}/index/movie/hot?qq=122212489&type=trailer`,
					method: 'POST',
					data: {},
					success: res => {
						this.trailer = res.data.data;
						// console.log(this.trailer)
						this.getlove()
					},
					fail: () => {},
					complete: () => {}
				});
			},
			//猜你喜欢 随机获取预告
			getlove() {
				uni.request({
					url: `${this.$api}/index/guessULike?qq=122212489`,
					method: 'POST',
					data: {},
					success: res => {
						this.love = res.data.data;
						console.log(res)

					},
					fail: () => {},
					complete: () => {}
				});
			},
			//赞一下
			toZan(){
				uni.showToast({
				    title: '已点赞',
				    duration: 2000
				});
			},
		},
		mounted() {

		},
		onLoad() {
			this.getslideshow()
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

<style>
	.slideshow_tp {
		width: 100%;
		height: 400rpx;
	}

	.slideshow_tp image {
		width: 100%;
		height: 100%;
	}

	.hot_ {
		display: flex;
		padding-left: 2%;
		height: 100rpx;
		line-height: 100rpx;
	}

	.hot_tp {
		width: 50rpx;
		height: 50rpx;
	}

	.hot_tp image {
		width: 100%;
		height: 100%;
	}

	.hot_title {
		width: 214rpx;
		/* line-height: 60rpx; */
		text-align: center;
		font-weight: bold;
	}

	.preview {
		display: flex;
	}

	.preview_tp {
		width: 300rpx;
		height: 366rpx;
	}

	.preview_tp image {
		width: 100%;
		height: 100%;
	}

	.preview_name {
		width: 300rpx;
		line-height: 60rpx;
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
	}

	.preview_xing {
		width: 300rpx;
		height: 80rpx;
		line-height: 80rpx;
		display: flex;

	}

	.preview_item {
		margin: 0 2%;

	}

	.preview_xing_x {
		padding-top: 38rpx;
	}

	.HomeData_vide {
		display: flex;
		flex-wrap: wrap;
	}

	.vide_k {
		width: 348rpx;
		height: 250rpx;
		margin: 1% auto;
	}

	.vide_k video {
		width: 100%;
		height: 100%;
	}

	.love_item {
		display: flex;
		justify-content: space-between;
		margin: 4% 0;
		padding: 0 2%;
		height: 350rpx;
	}

	.love_item_all {
		display: flex;
		border-right: 1rpx dashed #cdcdcd ;
	}

	.love_item_all_tp {
		width: 350rpx;
		height: 350rpx;
	}

	.love_item_all_tp image {
		width: 100%;
		height: 100%;
	}

	.zan {
		width: 50rpx;
		height: 50rpx;
		margin: 0 auto;
	}

	.zan image {
		width: 100%;
		height: 100%;
	}

	.love_item_center {
		padding-left: 3%;
		height: 350rpx;
	}

	.love_item_center_name {
		/* font-weight: bold; */
		width: 100%;
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
		/* position: relative; */
		/* line-height: 80rpx; */
		/* font-weight: bold; */
		font-size: 32rpx;
	}

	.love_item_basicInfo {
		/* width: 80%; */
		flex-wrap: wrap;
		color: #cdcdcd;
		font-size: 34rpx;
	}

	.love_rig_txt {
		color: #FFE3B4;
		text-align: center;
	}

	.love_rig {
		width: 200rpx;
		line-height: 350rpx;
		height: 350rpx;
	}

	.love_xing {
		height: 60rpx;
	}

	.love_xing_ {
		padding-top: 30rpx;
	}

	.love_item_releaseDate {
		margin-top: 1%;
	}
</style>
