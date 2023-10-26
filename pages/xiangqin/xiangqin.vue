<template>
	<view class="container container25492" v-if="share">
		<view class="flex flex-wrap diygw-col-24 flex-direction-column flex-clz">
			<view class="diygw-col-24 text-clz"> {{share.title}} </view>
			<view class="flex flex-wrap diygw-col-24 flex1-clz">
				<view class="diygw-col-6"> 作者：{{share.author}} </view>
				<view class="diygw-col-8"> 发布人：{{nickname}} </view>
				<view class="diygw-col-5"> 积分：{{share.price}} </view>
				<view class="diygw-col-5"> 来源：{{isOriginal}} </view>
			</view>
			<view class="flex diygw-col-24 justify-center avatar-clz">
				<view class="diygw-avatar avatar-avatar xl bg-none">
					<image mode="aspectFit" class="diygw-avatar-img" :src="share.cover"></image>
				</view>
			</view>
			<view class="diygw-col-24 text1-clz"> {{share.downloadUrl}} </view>
			<view class="diygw-col-24 text2-clz"> {{share.summary}} </view>
			<view class="flex diygw-col-24">
				<button class="diygw-btn red radius-xs flex-sub margin-xs button-button-clz" @tap="paste">复制下载地址</button>
			</view>
		</view>
		<view class="clearfix"></view>
	</view>
</template>

<script>
	import{
		request
	}from '@/utils/request.js';
	import{
		SHARE_URL
	}from '@/utils/api.js';
	
	export default {
		data() {
			return {
				//用户全局信息
				userInfo: {},
				//页面传参
				globalOption: {},
				//自定义全局变量
				globalData: {},
				share: null,
				nickname: '',
				avatarUrl: '',
				id: 0,
				isOriginal:''
			};
		},
		onShow() {
			this.setCurrentPage(this);
		},
		onLoad(option) {
			this.setCurrentPage(this);
			this.id=option.id;
			this.getShare();
			
			if (option) {
				this.setData({
					globalOption: this.getOption(option)
				});
			}

			this.init();
		},
		methods: {
			async getShare(){
				let id=this.id;
				uni.showLoading({
					title: '加载中'
				});
				let res=await request(SHARE_URL+`/${id}`,'GET');
				setTimeout(()=>{
					uni.hideLoading();
				},500);
				console.log(res.data);
				this.share=res.data.share;
				this.nickname=res.data.nickname;
				this.avatarUrl=res.data.avatarUrl;
				
				 if (res.data.isOriginal) {
			        this.isOriginal= "原创";
			      } else {
			        this.isOriginal ="转载";
			      }
				  console.log(this.isOriginal);
			},
			paste(){
				uni.setClipboardData({
					data: this.share.downloadUrl,
					success:function(){
						uni.showToast({
							title:'下载地址已复制'
						});
					}
				});
			},
			
			// async init() {}
		}
	};
</script>

<style lang="scss" scoped>
	.flex-clz {
		padding-top: 14rpx;
		padding-left: 14rpx;
		padding-bottom: 14rpx;
		padding-right: 14rpx;
	}
	.text-clz {
		margin-left: 0rpx;
		font-weight: bold;
		width: calc(100% - 0rpx - 10rpx) !important;
		font-size: 26rpx !important;
		margin-top: 10rpx;
		margin-bottom: 8rpx;
		margin-right: 10rpx;
	}
	.flex1-clz {
		padding-top: 10rpx;
		padding-left: 10rpx;
		padding-bottom: 10rpx;
		padding-right: 10rpx;
	}
	.avatar-clz {
		text-align: center;
	}
	.avatar-avatar {
		width: 200px;
		height: 200px;
	}
	.text1-clz {
		padding-top: 10rpx;
		padding-left: 10rpx;
		font-size: 26rpx !important;
		padding-bottom: 10rpx;
		text-align: left;
		padding-right: 10rpx;
	}
	.text2-clz {
		padding-top: 10rpx;
		padding-left: 10rpx;
		font-size: 26rpx !important;
		padding-bottom: 10rpx;
		text-align: left;
		padding-right: 10rpx;
	}
	.button-button-clz {
		margin: 3px !important;
	}
	.container25492 {
		padding-left: 0px;
		padding-right: 0px;
	}
	.container25492 {
	}
</style>
