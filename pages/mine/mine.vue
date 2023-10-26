<template>
	<view class="container container25492 padding-top-lg">
		
		
		<view class="flex diygw-col-24 flex-direction-column flex-nowrap" v-if="!userInfo">
			<view class="flex diygw-col-24 justify-center">
				<view class="diygw-avatar xl radius bg-none">
					<image mode="aspectFit" class="diygw-avatar-img radius" src="/static//1.jpg"></image>
				</view>
			</view>
			<view class="diygw-col-24 text-clz diygw-text-md"> 李心怡 </view>
			<view class="diygw-col-24 text1-clz diygw-text-sm"> 积分：886 </view>
			<view class="flex flex-wrap diygw-col-24 justify-center flex1-clz">
				<view class="flex diygw-col-6">
					<button @tap="navigateTo" data-type="page" data-url="/pages/login/login" 
					class="diygw-btn red radius-xs flex-sub margin-xs button-button-clz">登录</button>
				</view>
			</view>
		</view>
		
		
		<view class="flex diygw-col-24 flex-direction-column flex-nowrap" v-else>
			<view class="flex diygw-col-24 justify-center">
				<view class="diygw-avatar xl radius bg-none">
					<image mode="aspectFit" class="diygw-avatar-img radius" :src="userInfo.avatarUrl"></image>
				</view>
			</view>
			<view class="diygw-col-24 text-clz diygw-text-md"> 李心怡 </view>
			<view class="diygw-col-24 text1-clz diygw-text-sm"> 积分：520 </view>
			<view class="flex flex-wrap diygw-col-24 justify-center flex1-clz">
				<view class="flex diygw-col-6">
					<button @tap="navigateTo" data-type="page" data-url="/pages/login/login" 
					class="diygw-btn red radius-xs flex-sub margin-xs button-button-clz">签到</button>
				</view>
				<view class="flex diygw-col-6 justify-center">
					<button @tap="navigateTo" data-type="page" data-url="/pages/login/login" 
					class="diygw-btn red radius-xs flex-sub margin-xs button1-button-clz">退出</button>
				</view>
			</view>
		</view>
		
		<view v-if="userInfo">
			<view class="flex flex-wrap diygw-col-24 flex2-clz">
				<view class="diygw-col-12 text2-clz diygw-text-xs"> 我的兑换 </view>
				<view @tap="navigateTo" data-type="page" data-url="/pages/duihuan/duihuan" class="diygw-col-12 text3-clz diygw-text-xs"> 👉 </view>
			</view>
			<view class="flex flex-wrap diygw-col-24 flex5-clz">
				<view class="diygw-col-12 text8-clz diygw-text-xs"> 积分明细 </view>
				<view @tap="navigateTo" data-type="page" data-url="/pages/jifen/jifen" class="diygw-col-12 text9-clz diygw-text-xs"> 👉 </view>
			</view>
			<view class="flex flex-wrap diygw-col-24 flex4-clz" @click="jumpToTouGao">
				<view class="diygw-col-12 text6-clz diygw-text-xs"> 我的投稿 </view>
				<view @tap="navigateTo" data-type="page" data-url="/pages/tougao/tougao" class="diygw-col-12 text7-clz diygw-text-xs"> 👉 </view>
			</view>
			<view class="flex flex-wrap diygw-col-24 flex3-clz"
			 v-if="userInfo.roles == 'admin'" @click="jumpToShenHe">
				<view class="diygw-col-12 text4-clz diygw-text-xs"> 审核投稿 </view>
				<view @tap="navigateTo" data-type="page" data-url="/pages/audit_list/audit_list" class="diygw-col-12 text5-clz diygw-text-xs"> 👉 </view>
			</view>
		</view>
		
		
		<view class="clearfix"></view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				//用户全局信息
				userInfo: null,
				//页面传参
				globalOption: {},
				//自定义全局变量
				globalData: {}
			};
		},
		onShow() {
			this.setCurrentPage(this);
			if(uni.getStorageSync('user')){
				this.userInfo=uni.getStorageSync('user');
			}
		},
		onLoad(option) {
			this.setCurrentPage(this);
			if (option) {
				this.setData({
					globalOption: this.getOption(option)
				});
			}

			this.init();
		},
		methods: {
			async init() {},
			logout(){
				this.userInfo = null;
				uni.clearStorageSync();
			},
			jumpToTouGao(){
				uni.navigateTo({
					url:'/pages//tougao/tougao'
				})
			},
			jumpToShenHe(){
				uni.navigateTo({
					url:'/pages//shenhe/shenhe'
				})
			},
			jumpToPriceDetail(){
				uni.navigateTo({
					url:'/pages/price_detail/price_detail'
				})
			},
			jumpToMyExchange(){
				uni.navigateTo({
					url:'/pages/my_exchange/my_exchange'
				})
			}
		}
	};
</script>

<style lang="scss" scoped>
	.text-clz {
		padding-top: 10rpx;
		padding-left: 10rpx;
		padding-bottom: 10rpx;
		text-align: center;
		padding-right: 10rpx;
	}
	.text1-clz {
		padding-top: 10rpx;
		padding-left: 10rpx;
		padding-bottom: 10rpx;
		text-align: center;
		padding-right: 10rpx;
	}
	.flex1-clz {
		padding-top: 10rpx;
		padding-left: 10rpx;
		padding-bottom: 10rpx;
		padding-right: 10rpx;
	}
	.button-button-clz {
		margin: 3px !important;
	}
	.button1-button-clz {
		margin: 3px !important;
	}
	.flex2-clz {
		padding-top: 10rpx;
		padding-left: 10rpx;
		padding-bottom: 10rpx;
		border-bottom: 2rpx solid #eee;
		padding-right: 10rpx;
	}
	.text2-clz {
		text-indent: 10rpx !important;
		letter-spacing: 2rpx !important;
		line-height: 2;
	}
	.text3-clz {
		text-indent: 10rpx !important;
		letter-spacing: 2rpx !important;
		line-height: 2;
		text-align: right;
	}
	.flex5-clz {
		padding-top: 10rpx;
		padding-left: 10rpx;
		padding-bottom: 10rpx;
		border-bottom: 2rpx solid #eee;
		padding-right: 10rpx;
	}
	.text8-clz {
		text-indent: 10rpx !important;
		letter-spacing: 2rpx !important;
		line-height: 2;
	}
	.text9-clz {
		text-indent: 10rpx !important;
		letter-spacing: 2rpx !important;
		line-height: 2;
		text-align: right;
	}
	.flex4-clz {
		padding-top: 10rpx;
		padding-left: 10rpx;
		padding-bottom: 10rpx;
		border-bottom: 2rpx solid #eee;
		padding-right: 10rpx;
	}
	.text6-clz {
		text-indent: 10rpx !important;
		letter-spacing: 2rpx !important;
		line-height: 2;
	}
	.text7-clz {
		text-indent: 10rpx !important;
		letter-spacing: 2rpx !important;
		line-height: 2;
		text-align: right;
	}
	.flex3-clz {
		padding-top: 10rpx;
		padding-left: 10rpx;
		padding-bottom: 10rpx;
		border-bottom: 2rpx solid #eee;
		padding-right: 10rpx;
	}
	.text4-clz {
		text-indent: 10rpx !important;
		letter-spacing: 2rpx !important;
		line-height: 2;
	}
	.text5-clz {
		text-indent: 10rpx !important;
		letter-spacing: 2rpx !important;
		line-height: 2;
		text-align: right;
	}
	.container25492 {
		padding-left: 0px;
		padding-right: 0px;
	}
	.container25492 {
	}
</style>
