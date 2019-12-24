<template>
	<view>
		<view class="box">
		   <swiper :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" class="swiper">
		       <block v-for="(item,index) in banner" :key="index">
		         <swiper-item class="banner">
		           <image :src="item.image" class="img" :lazy-load=true></image>
		         </swiper-item>
		       </block>
		     </swiper> 
		</view>
		<view class="title_hot">
			<image src="../../static/icos/hot.png" :lazy-load=true></image>
			<view class="font">热门英超</view>
		</view>
		<scroll-view scroll-x="true" style="width: 100%">
		  <view class="inner">
		    <view v-for="(item,index) in hot" :key="index" class="hot">
		      <view class="box" @click="goto(item.id)">
		        <image :src="item.cover"></image>
		        <view class="name">{{item.name}}</view>
		        <view class="score">
					<uni-rate :value="item.score/2" disabled="true" size="16"></uni-rate>
					{{item.score}}
				</view>
		      </view>
		    </view>
		  </view>
		</scroll-view>
		<view class="title_hot">
			<image src="../../static/icos/interest.png" :lazy-load=true></image>
			<view class="font">热门预告</view>
		</view>
		<view class="cover">
			<view v-for="(item,index) in trailer" :key="index" class="box">
				<video :src="item.trailer" controls duration :poster="item.cover" class="video"></video>
			</view>
		</view>
		<view v-for="(item,index) in likes" :key="index" class="vfor">
			<view class="box" @click="goto(item.id)">
				<view class="img">
					<image :src="item.cover" :lazy-load=true class="cover"></image>	
				</view>
				<view class="center">
					<view class="name">{{item.name}}</view>
					<uni-rate :value="item.score/2" disabled="true" size="16" class="star"></uni-rate>
					<view class="basicInfo">{{item.basicInfo}}</view>
					<view class="releaseDate">{{item.releaseDate}}</view>
				</view>
				<view class="line"></view>
				<view class="right">
					<image src="../../static/icos/praise.png" :lazy-load=true @click="praise"></image>
					<view class="font" @click="praise">赞一下</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
import uniRate from '../../components/uni-ui/uni-rate/uni-rate.vue'
export default {
components: {
 uniRate
},
props: {},
data () {
  return {
	  banner:[],        //轮播图
	  hot:[],          //热门列表
	  trailer:[],       //预告
	  likes:[],         
	  indicatorDots: true,
	  vertical: true,
	  autoplay: true,
	  interval: 3000,
    }
  },
  methods: {
    getData(){               //轮播图数据
		uni.request({
			url: `${this.$api}/index/carousel/list?qq=122212489`,
			method: 'POST',
			data: {},
			success: res => {
				this.banner=res.data.data
			},
			fail: () => {},
			complete: () => {}
		});
	}, 
	getHot(){                     //热门列表数据
		uni.request({
			url: `${this.$api}/index/movie/hot?type=superhero&qq=122212489`,
			method: 'POST',
			data: {},
			success: res => {
				this.hot=res.data.data
			},
			fail: () => {},
			complete: () => {}
		});
	},
	gettrailer(){                //预告片数据
		uni.request({
			url: `${this.$api}/index/movie/hot?type=trailer&qq=122212489`,
			method: 'POST',
			data: {},
			success: res => {
				this.trailer=res.data.data
			},
			fail: () => {},
			complete: () => {}
		});
	},
	getlikes(){             //推荐影片数据
		uni.request({
			url: `${this.$api}/index/guessULike?qq=122212489`,
			method: 'POST',
			data: {},
			success: res => {
				console.log(res);
				this.likes=res.data.data
			},
			fail: () => {},
			complete: () => {}
		});
	},
	praise(){
		uni.showToast({            //点赞
			title:"已点赞"
		})
	},
	goto(id){                    //前往详情页
		uni.navigateTo({
			url:`/pages/detail/detail?id=${id}`
		})
	}
  },
  mounted () {
	  
  },
  onLoad () {
    this.getData()
	this.getHot()
	this.gettrailer()
	this.getlikes()
  },
  onPullDownRefresh() {
          console.log('refresh');             //下拉刷新
          this.getlikes()
		  setTimeout(function () {
		    uni.stopPullDownRefresh();
		  }, 1000);
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
  .box{
	width: 100%;
	.swiper{
		height: 400rpx;
		.banner{
		  width: 100%;
		  height: 400rpx;
		 .img{
		    width: 100%;
		    height: 100%;
		  }
		}
	}
  }
  .title_hot{
	  width: 95%;
	  padding: 20rpx 2.5%;
	  height: 80rpx;
	  display: flex;
	  align-items: center;
	  image{
		  width: 50rpx;
		  height: 50rpx;
	  }
	  .font{
		  margin-left: 40rpx;
	  }
  }
  .inner{
	  display: flex;
	  .hot{
		  display: inline-block;
		  height: 340rpx;
		  align-items: center;
		  .box{
			  height: 340rpx;
			  width: 200rpx;
			  margin: 0 20rpx;
			  image{
				  height: 240rpx;
				  width: 200rpx;
			  }
			  .name{
				  font-size: 28rpx;
				  line-height: 50rpx;
				  height: 50rpx;
				  overflow: hidden;
				  text-overflow: ellipsis;
				  white-space: nowrap;
			  }
			  .score{
				  display: flex;
				  align-items: center;
				  height: 50rpx;
				  line-height: 50rpx;
				  font-size: 28rpx;
				  .font{
					  height: 50rpx;
					  line-height: 50rpx;
					  font-size: 28rpx;
				  }
			  }
		  }
	  }
  }	  
  .cover{
	  display: flex;
	  flex-wrap: wrap;
	  width: 100%;
	  .box{
		  width: 46%;
		  height: 300rpx;
		  margin: 16rpx 2%;
		  .video{
			  width: 100%;
			  height: 100%;
		  }
	  }
  }
  .vfor{
	  width: 95%;
	  height: 300rpx;
	  margin: 40rpx 2.5%;
	  .box{
		width: 100%;  
		height: 300rpx;
		display: flex;
		.img{
			width: 240rpx;
			height: 300rpx;
			.cover{
				width: 240rpx;
				height: 300rpx;
			}
		}
		.center{
			margin-left: 20rpx;
			width: 320rpx;
			height: 300rpx;
			.name{
				width: 100%;
				font-size: 38rpx;
			}
			.star{
				width: 100%;
				margin: 10rpx 0;
			}
			.basicInfo{
				width: 100%;
				font-size: 28rpx;
				color: #999;
			}
			.releaseDate{
				width: 100%;
				font-size: 28rpx;
				color: #999;
			}
		}
		.line{
			margin-left: 20rpx;
			height: 300rpx;
			border: 4rpx dashed #f2f2f2;
		}
		.right{
			width: 100rpx;
			height: 300rpx;
			image{
				width: 50rpx;
				height: 50rpx;
				margin-left: 25rpx;
				margin-top: 100rpx;
			}
			.font{
				width: 100%;
				margin-top: 10rpx;
				text-align: center;
				font-size: 28rpx;
				color: #ffb655;
			}
		}
	  }
  }
</style>
