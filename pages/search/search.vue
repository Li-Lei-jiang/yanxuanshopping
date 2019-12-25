<template>
	<view>
		<view class="">
			<uni-search-bar @confirm="search" @input="input"></uni-search-bar>
		</view>
		<!-- 展示热门 -->
		<view class="searchHot_all">
			<view v-for="(item,index) in searchHot" :key="index" class="searchHot_item" @click="toItemDetai(item)">
				<view class="searchHot_tp">
					<image :src="item.cover" mode=""></image>
				</view>
				<view class="searchHot_name">{{item.name}}</view>
			</view>
		</view>









	</view>
</template>

<script>
	import uniSearchBar from '../../components/uni-ui/uni-search-bar/uni-search-bar.vue'
	export default {
		name: "",
		components: {
			uniSearchBar
		},
		props: {},
		data() {
			return {
				keywords: '', //搜索框的值
				searchHot: [], //搜索页热门

			}
		},
		methods: {
			//搜素事件
			search(e) {
				this.getSearchHot()
				// console.log(11);
			},
			input(e) { //
				this.keywords = e.value
				// console.log(this.keywords);
				// this.getSearchHot()
			},
			//获取搜素页热门预告
			getSearchHot() {
				let page = '1';
				let pageSize = 20;
				uni.request({
					url: `${this.$api}/search/list?qq=122212489&page=1&pageSize=${pageSize}&keywords=${this.keywords}`,
					method: 'POST',
					data: {},
					success: res => {
						this.searchHot = res.data.data.rows;
						// console.log(this.searchHot)
					},
					fail: () => {},
					complete: () => {}
				});
			},
			//去单个热门详情
			toItemDetai(e) {		
				uni.navigateTo({
					url: `/pages/search/hotDetails/hotDetails?id=`+e.id
				})
				console.log(e)
			},
			
		},

		mounted() {

		},
		onLoad() {
			this.getSearchHot()
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
	.searchHot_tp {
		width: 218rpx;
		height: 262rpx;
	}

	.searchHot_tp image {
		width: 100%;
		height: 100%;
	}

	.searchHot_all {
		display: flex;
		flex-wrap: wrap;
	}

	.searchHot_item {
		width: 218rpx;
		text-align: center;
		margin: 0 2%;
		height: 322rpx;
	}

	.searchHot_name {
		width: 218rpx;
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
		line-height: 60rpx;
	}
</style>
