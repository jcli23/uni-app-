<template>
	<view>
		<view class="all">
			<video :src="trailer.trailer" controls duration :poster="trailer.cover" class="video"></video>
			<image src="../../static/icos/fanhui.png" @click="back" class="back"></image>
			<view class="detail">
				<image :src="trailer.cover"></image>
				<view class="right">
					<view class="name">{{trailer.name}}</view>
					<view class="title">{{trailer.basicInfo}}</view>
					<view class="title">{{trailer.originalName}}</view>
					<view class="title">{{trailer.releaseDate}}</view>
					<view class="box">
						<view class="smallbox1">
							<view class="font">综合评分:</view>
							<view class="score">{{trailer.score}}</view>
						</view>
						<view class="smallbox2">
							<view class="rate">
								<uni-rate :value="trailer.score/2" disabled="true" size="16" class="unirate"></uni-rate>
							</view>
							<view class="zan">{{trailer.prisedCounts}}人点赞</view>
						</view>
					</view>
				</view>
			</view>
			<view class="plot">
				<view class="font">剧情介绍:</view>
				<view class="plotDesc">{{trailer.plotDesc}}</view>
			</view>
			<view class="personal">
				<view class="font">演职人员:</view>
				<scroll-view scroll-x="true" style="width: 100%">
				  <view class="actor">
				    <view v-for="(item,index) in actor" :key="index" class="for">
				      <view class="box">
				        <image :src="item.photo"></image>
				        <view class="name">{{item.name}}</view>
				        <view class="actName">{{item.actName}}</view>
				      </view>
				    </view>
					<view v-for="(item,index) in actorer" :key="index" class="for">
					  <view class="box">
					    <image :src="item.photo"></image>
					    <view class="name">{{item.name}}</view>
					    <view class="actName">{{item.actName}}</view>
					  </view>
					</view>
				  </view>
				</scroll-view>
			</view> 
			<view class="stills">
				<view class="font">剧照:</view>
				<view class="photos">
					<view v-for="(item,index) in plotPics" :key="index" class="for">
						<image :src="item"></image>
					</view>
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
				id: '',               //影片id
				trailer: {},           //预告片详情数据
				actor:[],              //导演
				actorer:[],           //演员
				plotPics:[]           //剧照
			}
		},
		methods: {          
			back() {                       //返回
				uni.navigateBack({
					delta: 1
				});
			},
			gettrailer(trailerId) {              //影片详情
				uni.request({
					url: `${this.$api}/search/trailer/${trailerId}?qq=122212489`,
					method: 'POST',
					data: {
						trailerId
					},
					success: res => {
						this.trailer = res.data.data
						this.plotPics=JSON.parse(res.data.data.plotPics)
						console.log(this.trailer);
					},
					fail: () => {},
					complete: () => {}
				});
			},
			getstaff1() {                 //导演
				uni.request({
					url: `${this.$api}/search/staff/${this.id}/${1}?qq=122212489`,
					method: 'POST',
					data: {
						
					},
					success: res => {
						this.actor=res.data.data
						console.log(this.actor);
					},
					fail: () => {},
					complete: () => {}
				});
			}, 
			getstaff2() {                 //演员
				uni.request({
					url: `${this.$api}/search/staff/${this.id}/${2}?qq=122212489`,
					method: 'POST',
					data: {
						
					},
					success: res => {
						this.actorer=res.data.data
						console.log(res);
					},
					fail: () => {},
					complete: () => {}
				});
			}
		},
		mounted() {

		},
		onLoad(options) {
			this.id = options.id
			this.gettrailer(this.id)
			this.getstaff1()
			this.getstaff2()
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
	.all {
		background: #f2f2f2;
		.back {
			position: absolute;
			width: 80rpx;
			height: 80rpx;
			top: 10rpx;
			left: 10rpx;
			z-index: 100;
		}

		.video {
			width: 100%;
			height: 500rpx;
		}

		.detail {
			width: 92%;
			height: 400rpx;
			margin: 0 4%;
			padding: 100rpx 0;
			display: flex;
			justify-content: space-between;
			border-bottom: 2rpx solid #333;

			image {
				width: 35%;
				height: 400rpx;
			}

			.right {
				width: 60%;
				height: 400rpx;

				.name {
					font-size: 40rpx;
					font-weight: 700;
					height: 80rpx;
					line-height: 80rpx;
					overflow: hidden;
					text-overflow: ellipsis;
					white-space: nowrap;
				}

				.title {
					height: 40rpx;
					line-height: 40rpx;
					font-size: 28rpx;
					color: #999;
					overflow: hidden;
					text-overflow: ellipsis;
					white-space: nowrap;
				}

				.box {
					width: 100%;
					height: 160rpx;
					background: white;
					margin-top: 40rpx;
					display: flex;

					.smallbox1 {
						width: 50%;
						height: 160rpx;

						.font {
							font-size: 40rpx;
							height: 80rpx;
							line-height: 80rpx;
							text-align: center;
						}

						.score {
							height: 80rpx;
							text-align: center;
							color: #ffb655;
						}
					}

					.smallbox2 {
						width: 50%;
						height: 160rpx;

						.rate {
							height: 40rpx;
							line-height: 40rpx;

							.unirate {
								margin-top: 45rpx;
							}
						}

						.zan {
							font-size: 32rpx;
							height: 45rpx;
							line-height: 45rpx;
						}
					}
				}
			}
		}

		.plot {
			width: 92%;
			margin: 0 4%;
			padding: 40rpx 0;
			border-bottom: 2rpx solid #333;

			.font {
				font-size: 28rpx;
				color: #999;
				line-height: 70rpx;
				height: 70rpx;
			}

			.plotDesc {
				font-size: 32rpx;
			}
		}
		.personal{
			width: 92%;
			margin: 0 4%;
			border-bottom: 2rpx solid #333;
			.font{
				font-size: 28rpx;
				color: #999;
				line-height: 70rpx;
				height: 70rpx;
			}
			.actor{
				display: flex;
				.for{
					display: inline-block;
					height: 400rpx;
					align-items: center;
					.box{
						width: 200rpx;
						height: 350rpx;
						margin: 0 16rpx;
						image{
							width: 200rpx;
							height: 260rpx;
						}
						.name{
							font-size: 36rpx;
							line-height: 50rpx;
							overflow: hidden;
							text-overflow: ellipsis;
							white-space: nowrap;
						}
						.actName{
							height: 40rpx;
							font-size: 28rpx;
							line-height:40rpx;
							color: #999;
							overflow: hidden;
							text-overflow: ellipsis;
							white-space: nowrap;
						}
					}
				}
				
			}
		}
		.stills{
			width: 92%;
			margin: 0 4%;
			border-bottom: 2rpx solid #333;
			.font{
				font-size: 28rpx;
				color: #999;
				line-height: 70rpx;
				height: 70rpx;
			}
			.photos{
				width: 100%;
				display: flex;
				flex-wrap: wrap;
				.for{
					width: 29%;
					height: 260rpx;
					margin: 10rpx 2%;
					image{
						width: 100%;
						height: 100%;
					}
				}
			}
		}
	}
</style>
