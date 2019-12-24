uyjhjhuy87<template>
    <view>
		<view class="background" v-if="user">
			<image :src="user.faceImage" ></image>
			<view class="font">{{user.nickname}}</view>
			<image src="../../static/icos/settings.png" class="edit" @click="edit"></image>
		</view>
		<view class="background" v-else-if="qquser">
			<image :src="qquser.avatarUrl" ></image>
			<view class="font">{{qquser.nickname}}</view>
			<image src="../../static/icos/settings.png" class="edit" @click="edit"></image>
			<view class="out" @click="outqq">退出登录</view>
		</view>
		<view class="background" v-else-if="weixinuser">
			<image :src="weixinuser.avatarUrl" ></image>
			<view class="font">{{weixinuser.nickName}}</view>
			<image src="../../static/icos/settings.png" class="edit" @click="edit"></image>
			<view class="out" @click="outweixin">退出登录</view>
		</view>
		<view class="background" v-else-if="weibouser">
			<image :src="weibouser.avatarUrl" ></image>
			<view class="font">{{weibouser.nickName}}</view>
			<image src="../../static/icos/settings.png" class="edit" @click="edit"></image>
			<view class="out" @click="outweibo">退出登录</view>
		</view>
		<view class="background" v-else>
			<image src="../../static/icos/default-face.png" ></image>
			<view class="font" @click="goto">注册/登录</view>
		</view>
    </view>
	
</template>

<script>
export default {
name: "",
components: {

},
props: {},
data () {
  return {
	 user:{},
	 qquser:{},
	 weixinuser:{},
	 weibouser:{}
    }
  },
  methods: {
    goto(){                     
		uni.navigateTo({
			url:'/pages/login/login'
		})
	},
	edit(){                        //前往编辑页
		uni.navigateTo({
			url:'/pages/edited/edited'
		})
	},
	outqq() {                       //退出QQ登录
	     uni.removeStorageSync('qquser')
	},
	outweixin() {                   //退出微信登录
	     uni.removeStorageSync('weixinuser')
	},
	outweibo() {                   //退出微博登录
	     uni.removeStorageSync('weibouser')
	}
  },
  mounted () {
    
  },
  onLoad () {
	this.user=uni.getStorageSync('user')
	this.qquser=uni.getStorageSync('qquser')
	this.weixinuser=uni.getStorageSync('weixinuser')
	this.weibouser=uni.getStorageSync('weibouser')
  },
  onShow() {
  	this.user=uni.getStorageSync('user')
	this.qquser=uni.getStorageSync('qquser')
	this.weixinuser=uni.getStorageSync('weixinuser')
	this.weibouser=uni.getStorageSync('weibouser')
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
  .background{
	  width: 100%;
	  height: 340rpx;
	  background: url(../../static/icos/bg.png) #ffe655 repeat 100% 100%;
	  background-size:250rpx 240rpx ;
	  display: flex;
	  image{
		  width: 140rpx;
		  height: 140rpx;
		  margin-left: 30rpx;
		  margin-top: 140rpx;
		  border-radius: 50%;
	  }
	  .font{
		  width: 400rpx;
		  height: 140rpx;
		  line-height: 140rpx;
		  margin-left: 50rpx;
		  font-size: 38rpx;
		  margin-top: 140rpx;
	  }
	  .edit{
		  margin-top: 180rpx;
		  width: 60rpx;
		  height: 60rpx;
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
  }
</style>
