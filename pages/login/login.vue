<template>
	<view>
		<!-- #ifdef H5 || APP-PLUS -->
		<view class="button">
			<image src="/static/icos/fanhui.png" @click="back"></image>
		</view>
		<!-- #endif -->
		<image src="../../static/icos/default-face.png" class="cover"></image>
		<view class="center">
			<view class="input">
				<view class="font">账户</view>
				<input type="text" class="value" @blur="getuser" placeholder="请输入用户名" />
			</view>
			<view class="input">
				<view class="font">密码</view>
				<input type="password" class="value" @blur="getpassword" placeholder="请输入密码" />
			</view>
		</view>
		<view class="login" @click="login">注册/登录</view>
		<!-- #ifdef APP-PLUS -->
		<view class="three">第三方登录</view>
		<view class="icon">
			<image src="../../static/icos/QQ.png" @click="qqlogin"></image>
			<image src="../../static/icos/weixin.png" @click="weixinlogin"></image>
			<image src="../../static/icos/weibo.png" @click="weibologin"></image>
		</view>
		<!-- #endif -->
		<!-- #ifdef MP-WEIXIN -->
		<view class="three">第三方登录</view>
		<view class="icon">
			<image src="../../static/icos/weixin.png"></image>
		</view>
		<!-- #endif -->
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
				username: '',
				password: '',
				loginRes:{}
			}
		},
		methods: {
			back() {                   //返回我的
				uni.switchTab({
					url: "/pages/me/me"
				})
			},
			getuser(e) {               //获得用户名
				this.username = e.detail.value
			},
			getpassword(e) {              //获得密码值
				this.password = e.detail.value
			},
			login() {                 //注册加登录
				uni.request({
					url: `${this.$api}/user/registOrLogin?qq=122212489`,
					method: 'POST',
					data: {
						password: this.password,
						username: this.username
					},
					success: res => {
						console.log(res);
						uni.setStorageSync('user', res.data.data)
						uni.switchTab({
							url: "/pages/me/me"
						})
					},
					fail: () => {},
					complete: () => {}
				});
			},
			qqlogin() {                   //app端第三方QQ登录
				uni.getProvider({
					service: 'oauth',
					success: res => {
						uni.login({
							provider: 'qq',
							success: loginRes=> {
								if(loginRes.authResult){
									uni.getUserInfo({
									      provider: 'qq',
									      success: infoRes=>{
									        console.log(infoRes);
											uni.setStorageSync('qquser',infoRes.userInfo)
											uni.switchTab({
												url: "/pages/me/me"
											})
									      }
									    });
								}
							}
						});

					}
				});
			},
			weixinlogin() {                  //app端第三方微信登录
				uni.getProvider({
					service: 'oauth',
					success: res => {
						uni.login({
							provider: 'weixin',
							success: loginRes=> {
								if(loginRes.authResult){
									uni.getUserInfo({
									      provider: 'weixin',
									      success: infoRes=>{
									        console.log(infoRes);
											uni.setStorageSync('weixinuser',infoRes.userInfo)
											uni.switchTab({
												url: "/pages/me/me"
											})
									      }
									    });
								}
							}
						});
			
					}
				});
			},
			weibologin() {                           //app端第三方微博登录
				uni.getProvider({
					service: 'oauth',
					success: res => {
						uni.login({
							provider: 'sinaweibo',
							success: loginRes=> {
								if(loginRes.authResult){
									uni.getUserInfo({
									      provider: 'sinaweibo',
									      success: infoRes=>{
									        console.log(infoRes);
											uni.setStorageSync('weibouser',infoRes.userInfo)
											uni.switchTab({
												url: "/pages/me/me"
											})
									      }
									    });
								}
							}
						});
			
					}
				});
			}
		},
		mounted() {

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
	.button {
		position: absolute;
		width: 80rpx;
		height: 80rpx;
		top: 0;
		left: 0;

		image {
			width: 100%;
			height: 100%;
		}
	}

	.cover {
		width: 140rpx;
		height: 140rpx;
		margin-left: 50%;
		position: relative;
		left: -70rpx;
		margin-top: 200rpx;
	}

	.center {
		width: 90%;
		padding: 0 5%;
		margin-top: 100rpx;

		.input {
			display: flex;
			height: 100rpx;
			align-items: center;
			border-bottom: 2rpx solid #f2f2f2;

			.font {
				font-size: 40rpx;
			}

			.value {
				margin-left: 40rpx;
				height: 100rpx;
			}
		}
	}

	.login {
		width: 80%;
		height: 90rpx;
		margin: 40rpx 10% 0 10%;
		text-align: center;
		line-height: 90rpx;
		color: white;
		border-radius: 10rpx;
		background: blue;
	}

	// #ifdef MP-WEIXIN 
	.login {
		width: 80%;
		height: 90rpx;
		border-radius: 10rpx;
		margin: 40rpx 10% 0 10%;
		text-align: center;
		line-height: 90rpx;
		color: white;
		background: green;
	}

	// #endif 
	.three {
		font-size: 28rpx;
		color: #999;
		text-align: center;
		margin-top: 50rpx;
	}

	.icon {
		margin-top: 20rpx;
		display: flex;
		justify-content: center;

		image {
			width: 70rpx;
			height: 70rpx;
			padding: 0 60rpx;
		}
	}
</style>
