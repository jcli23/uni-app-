<template>
	<view>
		<view class="top">
			<uni-search-bar @confirm="search" @cancel="cancel" placeholder="请输入搜索关键字" :radius="100" class="search"></uni-search-bar>
		</view>
		<scroll-view scroll-y="true" style="height: 1200rpx;" @scrolltolower="refresh">
			<view class="movies">
				<view v-for="(item,index) in rows" :key="index" class="rows">
					<view class="box" @click="goto(item.id)">
						<image :src="item.cover" :lazy-load=true></image>
						<view class="name">{{item.name}}</view>
					</view>
				</view>
			</view>
		</scroll-view>
	</view>
</template>

<script>
	import uniSearchBar from '../../components/uni-ui/uni-search-bar/uni-search-bar.vue'
	export default {

		components: {
			uniSearchBar
		},
		props: {},
		data() {
			return {
				page: 1,
				pageSize: 12,
				keywords: '',
				rows: []
			}
		},
		methods: {
			gethot() {
				uni.request({
					url: `${this.$api}/search/list?keywords=${this.keywords}&qq=122212489&page=${this.page}&pageSize=${this.pageSize}`,
					method: 'POST',
					data: {},
					success: res => {
						console.log(res);
						this.rows = res.data.data.rows
						console.log(this.rows);
					},
					fail: () => {},
					complete: () => {}
				});
			},
			search(e) {
				this.keywords = e.value
				this.gethot()
			},
			cancel(e) {
				this.keywords = ''
				this.gethot()
			},
			refresh() {
				this.pageSize += 12
				this.gethot()
			},
			goto(id) {
				uni.navigateTo({
					url: `/pages/detail/detail?id=${id}`
				})
			}
		},
		mounted() {
			this.gethot()
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
	.top {
		height: 100rpx;
		border-bottom: 2rpx solid #f2f2f2;

		.search {
			height: 100rpx;
		}
	}

	.movies {
		width: 100%;
		display: flex;
		flex-wrap: wrap;

		.rows {
			width: 29%;
			height: 380rpx;
			margin: 20rpx 2%;

			.box {
				width: 100%;
				height: 380rpx;

				image {
					width: 100%;
					height: 280rpx;
				}

				.name {
					height: 100rpx;
					line-height: 50rpx;
					display: -webkit-box;
					-webkit-box-orient: vertical;
					-webkit-line-clamp: 2;
					overflow: hidden;
				}
			}
		}
	}
</style>
