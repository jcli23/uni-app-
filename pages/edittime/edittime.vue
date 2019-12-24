<template>
	<view class="all">
		<view class="top">
			<image src="../../static/icos/fanhui.png" @click="back"></image>
			<view class="font">修改生日</view>
		</view>
		<view class="input" @click="open">
			<picker mode="date" :value="date" :start="startDate" :end="endDate" @change="bindDateChange">
				<view class="uni-input">{{date}}</view>
			</picker>
		</view>
		<view class="edit" @click="edittime">提交</view>
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
			const currentDate = this.getDate({
				format: true
			})
			return {
				user: {},
				date: currentDate
			}
		},
		methods: {
			back(){                //返回编辑页
					uni.navigateTo({
						url:"/pages/edited/edited"
					})  
			},
			open() {                //打开时间选择器
				this.$refs.popup.open()
			},
			getDate(type) {              //引用的组件内容
				const date = new Date();
				let year = date.getFullYear();
				let month = date.getMonth() + 1;
				let day = date.getDate();

				if (type === 'start') {
					year = year - 60;
				} else if (type === 'end') {
					year = year + 2;
				}
				month = month > 9 ? month : '0' + month;;
				day = day > 9 ? day : '0' + day;
				return `${year}-${month}-${day}`;
			},
			bindDateChange: function(e) {            //选择时间事件
				this.date = e.target.value
			},
			edittime() {                        //修改生日
				this.user.birthday=this.date
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
						uni.navigateTo({
							url: "/pages/edited/edited"
						})
					},
					fail: () => {},
					complete: () => {}
				});
			},

		},
		mounted() {

		},
		onLoad() {
			this.user = uni.getStorageSync('user')
		},
		filters: {

		},
		computed: {
			startDate() {
				return this.getDate('start');
			},
			endDate() {
				return this.getDate('end');
			}
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

		.uni-input {
			height: 100rpx;
			width: 100%;
			line-height: 100rpx;
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
