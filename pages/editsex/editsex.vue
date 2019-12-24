<template>
	<view class="all">
		<view class="top">
			<image src="../../static/icos/fanhui.png" @click="back"></image>
			<view class="font">修改性别</view>
		</view>
		<view class="choose">
			<view class="left">
				<view class="circular" :class="choose? 'bg':''" @click="choosesex(true)"></view>
				<view class="font">男</view>
			</view>
			<view class="right">
				<view class="circular" :class="!choose? 'bg':''" @click="choosesex(false)"></view>
				<view class="font">女</view>
			</view>
		</view>
		<view class="edit" @click="editsex">提交</view>
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
				user: {},
				choose:null        //选择值
			}
		},
		methods: {                //返回编辑页
			back() {
				uni.navigateTo({
					url: "/pages/edited/edited"
				})
			},
			choosesex(e){              //点击货值
				this.choose=e
			},
			editsex(){                      //修改性别
				if(this.choose===true){
					this.user.sex=0
				}else{
					this.user.sex=1
				}
				console.log(this.user.sex)
				uni.request({
					url: `${this.$api}/user/modifyUserinfo?qq=122212489`,
					method: 'POST',
					header: {
						"headerUserId": this.user.id,
						"headerUserToken": this.user.userUniqueToken
					},
					data: {
						birthday:this.user.birthday,
						nickname:this.user.nickname,
						sex:this.user.sex,
						userId:this.user.id
					},
					success: res => {
						console.log(res);
						uni.setStorageSync('user',this.user)
						uni.navigateTo({
							url:"/pages/edited/edited"
						})
					},
					fail: () => {},
					complete: () => {}
				});
			}
		},
		mounted() {

		},
		onLoad() {
			this.user = uni.getStorageSync('user')
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
		position: absolute;
		top: 0;
		bottom: 0;
		left: 0;
		right: 0;
		background: #f2f2f2;
	}

	.top {
		width: 100%;
		height: 80rpx;
		background: #f2f2f2;
		position: relative;

		.font {
			width: 100%;
			height: 80rpx;
			line-height: 80rpx;
			text-align: center;
		}

		image {
			position: absolute;
			width: 60rpx;
			height: 60rpx;
			top: 10rpx;
			left: 10rpx;
		}
	}

	.choose {
		height: 80rpx;
		display: flex;
		align-items: center;
		background: white;

		.left {
			width: 100rpx;
			height: 80rpx;
			display: flex;
			align-items: center;
			justify-content: space-between;
			margin-left: 40rpx;

			.circular {
				width: 50rpx;
				height: 50rpx;
				border-radius: 50%;
				border: 2rpx solid black;
			}
			.bg{
				background: blue;
			}

			.font {
				font-size: 40rpx;
			}
		}

		.right {
			width: 100rpx;
			height: 80rpx;
			display: flex;
			align-items: center;
			justify-content: space-between;
			margin-left: 40rpx;

			.circular {
				width: 50rpx;
				height: 50rpx;
				border-radius: 50%;
				border: 2rpx solid black;
			}
			.bg{
				background: blue;
			}

			.font {
				font-size: 40rpx;
			}
		}
	}

	.edit {
		height: 100rpx;
		width: 90%;
		margin: 20rpx 5% 0 5%;
		background: blue;
		border-radius: 20rpx;
		color: white;
		text-align: center;
		line-height: 100rpx;
		font-size: 36rpx;
	}
</style>
