<template>
	<view id="hotDeAll">
		<view class="hotdetails_video"><video :src="hotdetails.trailer" class="HomeData_vides"></video></view>
		<view class="hotdetails_hades">
			<view class="hotdetails_hades_tp">
				<image :src="hotdetails.poster" mode=""></image>
			</view>
			<view class="hotdetails_hades_rig">
				<view>{{hotdetails.name}}</view>
				<view class="hotdetails_to">{{hotdetails.basicInfo}}</view>
				<view class="hotdetails_to">{{hotdetails.originalName}}</view>
				<view class="hotdetails_to">{{hotdetails.releaseDate}}</view>
				<view class="hotdetails_hades_rig_zh">
					<view class="hotdetails_hades_rig_end">
						<view class="syntheticalMark">综合评分:</view>
						<view class="syntheticalMark_score">{{hotdetails.score}}</view>
					</view>
					<view>
						<view class="syntheticalMark_xing">
							<uni-rate disabled="true" :value="hotdetails.score/2" size="18" color="#ececec" class="xing"></uni-rate>
						</view>
						<view class="syntheticalMark_zan">{{hotdetails.prisedCounts}} 人点赞</view>
					</view>
				</view>
			</view>
		</view>
		<view class="hotdetails_center">
			<view class="hotdetails_center_jieshao">剧情介绍：</view>
			<view class="hotdetails_center_plotDesc">{{hotdetails.plotDesc}}</view>
		</view>
		<view class="centers">
			<view class="centers_participant">演职人员</view>
			<scroll-view scroll-x="true">
				<view class="participant_all">
					<view v-for="(item,index) in participant" :key="index" class="participant_item">
						<view class="participant_tp">
							<image :src="item.photo" mode=""></image>
						</view>
						<view class="item_name">{{item.name}}</view>
						<view class="item_name actName" v-if="index===0">{{item.actName}}</view>
						<view class="item_name actName" v-else>:饰演:{{item.actName}}</view>
					</view>
				</view>
			</scroll-view>

		</view>
		<view class="end">
			<view class="end_s">剧照</view>
			<view class="L0">
				<view v-for="(item,index) in stills" :key="index" class="stills_item">
					<view class="stills_tp">
						<image :src="item" mode=""></image>
					</view>
				</view>
			</view>
		</view>






	</view>
</template>

<script>
	import uniRate from '../../../components/uni-ui/uni-rate/uni-rate.vue'
	export default {
		name: "",
		components: {
			uniRate
		},
		props: {},
		data() {
			return {
				trailerId: '', //搜索热门传过来的id
				hotdetails: {}, //获取到的电影详情
				participant: [], //演职人员
				stills:[],//剧照
			}
		},
		methods: {
			getDetails() { //获取热门详情
				uni.request({
					url: `${this.$api}/search/trailer/${this.trailerId}?qq=122212489`,
					method: 'POST',
					data: {},
					success: res => {
						this.hotdetails = res.data.data;
						// console.log(this.hotdetails)
						this.getdirector()
						this.stills = JSON.parse(res.data.data.plotPics)
						// console.log(this.stills)
					},
					fail: () => {},
					complete: () => {}
				});
			},
			//获取导演
			getdirector() {
				uni.request({ //获取导演
					url: `${this.$api}/search/staff/${this.trailerId}/1?&qq=122212489`,
					method: 'POST',
					data: {},
					success: res => {
						res.data.data.map(item => {
							this.participant.push(item)
						})
						this.getactor()
					},
					fail: () => {},
					complete: () => {}
				});
			},
			//获取演员列表
			getactor() {
				// let actor = []				
				//获取演员
				uni.request({ //获取导演
					url: `${this.$api}/search/staff/${this.trailerId}/2?&qq=122212489`,
					method: 'POST',
					data: {},
					success: res => {
						res.data.data.map(item => {
							this.participant.push(item)
						})
					},
					fail: () => {},
					complete: () => {}
				});

				console.log(this.participant)
			},


		},

		mounted() {

		},
		onLoad(e) {
			this.trailerId = e.id
			// console.log(this.trailerId)
			this.getDetails()
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
	#hotDeAll {
		background: #F7F7F7;
	}

	.hotdetails_video {
		height: 500rpx;
		width: 100%;
	}

	.HomeData_vides {
		height: 100%;
		width: 100%;
	}

	.hotdetails_hades {
		display: flex;
		justify-content: space-between;
		padding: 0 3%;
		margin: 10% 0;
		border-bottom: 1rpx solid white;
	}

	.hotdetails_hades_tp {
		width: 268rpx;
		height: 374rpx;
	}

	.hotdetails_hades_tp image {
		width: 100%;
		height: 100%;
	}

	.hotdetails_hades_rig {
		padding-left: 4%;
	}

	.hotdetails_hades_rig_zh {
		display: flex;
		justify-content: space-between;
		height: 146rpx;
		text-align: center;
		background: white;
	}

	.hotdetails_to {
		font-size: 30rpx;
		color: #cdcdcd;
		line-height: 60prx;
		height: 60rpx;
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
	}

	.hotdetails_toname {
		height: 80rpx;
		line-height: 80rpx;
		font-size: 50rpx;
		font-weight: bold;
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
	}

	.syntheticalMark {
		font-size: 40rpx;
	}

	.syntheticalMark_score {
		font-size: 41rpx;
		color: #FFD655;
	}

	.syntheticalMark_xing {
		height: 50rpx;
	}

	.xing {
		padding-top: 36rpx;
	}

	.syntheticalMark_zan {
		font-size: 32rpx;
		color: #cdcdcd;
		height: 70rpx;
		line-height: 70rpx;
	}

	.hotdetails_center {
		width: 97%;
		margin: 3% auto;
		border-top: 10rpx solid #CCCCCC;
		// border-bottom: 4rpx solid #CCCCCC;
	}

	.hotdetails_center_jieshao {
		color: #DED4CC;
		font-size: 28rpx;
		line-height: 32px;
		padding-left: 2%;
	}

	.hotdetails_center_plotDesc {
		font-size: 28rpx;
		padding: 0 2%;
	}
	.participant_all{
		display: flex;
	}
	.centers{
		margin-top: 2%;
		border-top: 3rpx solid #C0C0C0;
		border-bottom: 3rpx solid #C0C0C0;
		width: 98%;
		margin: 3% auto;
	}
	.participant_tp{
		width: 246rpx;
		height: 280rpx;
	}
	.participant_tp image{
		width: 100%;
		height:100%;
	}
	.participant_item{
		width: 300rpx;
		margin: 0 1%;
	}
	.centers_participant{
		line-height: 80rpx;
		padding-left: 2%;
		// line-height: 60rpx;
		color: #C0C0C0;
	}
	.item_name{
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
		width: 246rpx;
		line-height: 60rpx;
		text-align: center;
	}
	.actName{
		color: #C0C0C0;
	}
	.end{
		padding: 0 3%;
		width: 97%;
		margin:  0 auto;
	}
	.end_s{
		line-height: 60rpx;
		color: #C0C0C0;
	}
	.stills_item{
		margin:2%;
	}
	.L0{
		display: flex;
		flex-wrap: wrap;
	}
	.stills_tp{
		width: 200rpx;
		height: 260rpx;
	}
	.stills_tp image{
		width: 100%;
		height:100%;
	}
</style>
