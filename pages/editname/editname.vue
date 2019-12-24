<template>
	<view class="all">
		<view class="top">
			<image src="../../static/icos/fanhui.png" @click="back"></image>
			<view class="font">修改昵称</view>
		</view>
		<view class="input">
			<input type="text" :value="user.nickname" class="value" @blur="edituser" />
		</view>
		<view class="edit" @click="editname">提交</view>
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
				user: {}
			}
		},
		methods: {
			back() {                         //返回编辑页
				uni.navigateTo({
					url: "/pages/edited/edited"
				})
			},
			edituser(e) { //改输入框值
				this.user.nickname = e.detail.value
			},
			editname() { //修改昵称
				uni.request({
					url: `${this.$api}/user/modifyUserinfo?qq=122212489`,
					method: 'POST',
					header: {
						"headerUserId": this.user.id,
						"headerUserToken": this.user.userUniqueToken
					},
					data: {
						birthday: this.user.birthday,
						nickname: this.user.nickname,
						sex: this.user.sex,
						userId: this.user.id
					},
					success: res => {
						console.log(res);
						uni.setStorageSync('user', this.user)
						uni.navigateTo({
							url: "/pages/edited/edited"
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

	.input {
		height: 100rpx;
		width: 90%;
		padding: 0 5%;
		background: white;

		.value {
			height: 100%;
			width: 100%;
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
