<template>
	<view class="all">
		<view class="cover" @click="open">
			<view class="left">
				<view class="font">头像</view>
				<image :src="user.faceImage" class="faceImage"></image>
			</view>
			<view class="icon">
				<image src="../../static/icos/left-gray-arrow.png"></image>
			</view>
		</view>
		<uni-popup ref="popup" type="bottom">
			<view class="eject">
				<view class="title" @click="see">查看我的头像</view>
				<view class="title" @click="chooseimage">从手机相册中选择</view>
				<view class="cancle" @click="cancle">取消</view>
			</view>
		</uni-popup>
		<view class="box" @click="editname">
			<view class="left">
				<view class="font">昵称</view>
				<view v-if="user.nickname!==null" class="title">{{user.nickname}}</view>
			</view>
			<view class="icon">
				<image src="../../static/icos/left-gray-arrow.png"></image>
			</view>
		</view>
		<view class="box" @click="edittime">
			<view class="left">
				<view class="font">生日</view>
				<view v-if="user.birthday!==null" class="title">{{user.birthday}}</view>
			</view>
			<view class="icon">
				<image src="../../static/icos/left-gray-arrow.png"></image>
			</view>
		</view>
		<view class="box" @click="editsex">
			<view class="left">
				<view class="font">性别</view>
				<view v-if="user.sex===0" class="title">男</view>
				<view v-else class="title">女</view>
			</view>
			<view class="icon">
				<image src="../../static/icos/left-gray-arrow.png"></image>
			</view>
		</view>
		<view class="preservation" @click="preservation">保存</view>
		<view class="out" @click="out">退出登录</view>
	</view>
</template>

<script>
	import uniPopup from "../../components/uni-ui/uni-popup/uni-popup.vue"
	export default {
		name: "",
		components: {
			uniPopup
		},
		props: {},
		data() {
			return {
				user: {}              
			}
		},
		methods: {
			open() {
				// 需要在 popup 组件，指定 ref 为 popup
				this.$refs.popup.open()
			},
			cancle() {
				// 需要在 popup 组件，指定 ref 为 popup
				this.$refs.popup.close()
			},
			see() {
				uni.previewImage({
					urls: [this.user.faceImage]
				});
			},
			chooseimage() {                //上传头像
				uni.chooseImage({
					sourceType: ['album'],
					success: res => {
						this.user.faceImage = res.tempFilePaths[0]
						this.$refs.popup.close()    //关闭弹出层
						uni.request({
							url: `${this.$api}/user/modifyUserinfo?qq=122212489`,
							method: 'POST',
							header: {
								"headerUserId": this.user.id,
								"headerUserToken": this.user.userUniqueToken
							},
							data: {
								birthday: this.date,
								nickname: this.user.nickname,
								sex: this.user.sex,
								userId: this.user.id
							},
							success: res => {
								console.log(res);
								uni.setStorageSync('user', this.user)
							},
							fail: () => {},
							complete: () => {}
						});
					},
				})
			},
			editname() {
				uni.navigateTo({                      //前往编辑昵称
					url: '/pages/editname/editname',
				});
			},
			edittime() {                        //前往修改生日
				uni.navigateTo({
					url: '/pages/edittime/edittime',
				});
			},
			editsex() {                      //前往修改性别
				uni.navigateTo({
					url: '/pages/editsex/editsex',
				});
			},
			preservation(){                  //保存和跳转
				uni.showToast({
					title:'保存成功'
				})
				uni.switchTab({
					url: "/pages/me/me"
				})
			},
			out() {                       //退出登录
				uni.request({
					url: `${this.$api}/user/logout?userId=${this.user.id}&qq=122212489`,
					method: 'POST',
					data: {},
					success: res => {
						console.log(res);
						uni.removeStorageSync('user')
						uni.switchTab({
							url: "/pages/me/me"
						})
					},
					fail: res => {
						console.log(res);
					},
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
		background: #f2f2f2;
		position: absolute;
		left: 0;
		right: 0;
		top: 0;
		bottom: 0;

		.cover {
			width: 95%;
			padding: 0 2.5%;
			height: 120rpx;
			display: flex;
			align-items: center;
			background: white;
		}

		.box {
			width: 95%;
			padding: 0 2.5%;
			height: 80rpx;
			display: flex;
			align-items: center;
			background: white;
		}

		.left {
			width: 90%;
			display: flex;
			align-items: center;
			justify-content: space-between;

			image {
				width: 80rpx;
				height: 80rpx;
				border-radius: 50%;
			}

			.font {
				font-size: 36rpx;
			}

			.title {
				font-size: 36rpx;
				color: #999;
			}
		}

		.icon {
			width: 10%;
			height: 100%;
			display: flex;
			align-items: center;
			justify-content: center;

			image {
				width: 30rpx;
				height: 40rpx;
			}
		}
        .preservation{
        	position: fixed;
        	bottom: 120rpx;
        	width: 100%;
        	height: 120rpx;
        	line-height: 120rpx;
        	text-align: center;
        	background: white;
        	font-size: 36rpx;
			border-bottom: 2rpx solid #f2f2f2;
        }
		.out {
			position: fixed;
			bottom: 0;
			width: 100%;
			height: 120rpx;
			line-height: 120rpx;
			text-align: center;
			background: white;
			font-size: 36rpx;
		}

		.eject {
			width: 100%;
			height: 400rpx;
			background: #f2f2f2;

			.title {
				height: 120rpx;
				line-height: 120rpx;
				width: 100%;
				text-align: center;
				background: white;
				border: 2rpx solid #f2f2f2;
				font-size: 40rpx;
			}
			.cancle {
				height: 120rpx;
				line-height: 120rpx;
				width: 100%;
				text-align: center;
				background: white;
				border: 2rpx solid #f2f2f2;
				margin-top: 30rpx;
				font-size: 40rpx;
			}
		}
	}
</style>
