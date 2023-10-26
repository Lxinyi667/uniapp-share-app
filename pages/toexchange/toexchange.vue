<template>
	<view class="container container25365" >
		<view v-if="share!=null">
			<view class="flex flex-wrap diygw-col-24 flex-direction-column flex-clz">
				<view class="diygw-col-24 text-clz"> {{share.title}} </view>
				<view class="flex flex-wrap diygw-col-24 flex1-clz">
					 作者：{{share.author}} 发布人：{{nickname}} 下载次数：{{share.buyCount}}
				</view>
				<image :src="share.cover" class="image-size diygw-image diygw-col-24 image-clz" mode="aspectFtt"></image>
				<view class="diygw-col-24 text2-clz"> {{share.summary}} </view>
				<view class="flex diygw-col-24 justify-end items-center flex-nowrap flex-cl">
					<text style="font-size:l4px;margin-right:10px;">积分：￥{{share.price}}</text>
					<view class="flex diygw-col-7 button-clz">
							<button class="diygw-btn red raidus-xs flex-sub margin-xs button-button-clz" @tap="exchange">
								兑换</button>
					</view>
				</view>
				
				
			</view>
			<view class="clearfix"></view>
		</view>
		<view v-else>
			<view class="flex flex-wrap diygw-col-24 flex-direction-column flex-clz">
				<view class="diygw-col-24 text-clz">0</view>
				<view class="flex flex-wrap diygw-col-24 flex1-clz">
					0
				</view>
				<image :src="share.cover" class="image-size diygw-image diygw-col-24 image-clz" mode="aspectFtt"></image>
				<view class="flex diygw-col-24 justify-end items-center flex-nowrap zidingyi">
					<text style="font-size:l4px;margin-right:10px;">积分：￥0</text>
					<view class="flex diygw-col-7 button-clz">
							<button class="diygw-btn red raidus-xs flex-sub margin-xs button-button-clz" @tap="exchange">
								兑换</button>
					</view>
				</view>
				<view class="diygw-col-24 text2-clz"> 0 </view>
				
			</view>
			<view class="clearfix"></view>
		</view>
	</view>
</template>

<script>
	import {request} from '@/utils/request.js';
	import {SHARE_URL,SHARE_EXCHANGE_URL} from '@/utils/api.js';
	
	export default {
		data() {
			return {
				share:null,
				nickname:'',
				avatarUrl:'',
				id:'',
			};
		},
	onShow() {
		
	},
	onLoad(option) {
		this.id = option.id;
		console.log(this.id);
		this.getShare();
	},
		methods: {
			async getShare(){
				let id = this.id;
				uni.showLoading({
					title:'加载中'
				});
				let res = await request(SHARE_URL + `/${id}`,'GET');
				setTimeout(()=>{
					uni.hideLoading();
				},500);
				console.log(res.data);
				this.share = res.data.share;
				console.log(this.share);
				this.nickname = res.data.nickname;
				this.avatarUrl = res.data.avatarUrl;
			},
			async exchange(){
				let shareId = this.id;
				const userId = uni.getStorageSync('userInfo').id;
				const res = await request(SHARE_EXCHANGE_URL,"POST",{
					userId:userId,
					shareId:shareId
				});
				console.log(res);
				if(res.success){
					uni.showModal({
						title:'兑换成功，是否跳转下载界面',
						success(res){
							console.log(shareId);
							if(res.confirm){
								uni.redirectTo({
									url:`/pages/detail_d/detail_d?id=${shareId}`
								})
							};
							if(res.cancel){
								uni.switchTab({
									url:'/pages/index/index'
								})
							}

						},
					});

				}else{
					uni.showToast({
						title:'兑换失败',
						icon:'error'
					});
				}
			}
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
	.flex-cl{
		padding-top: 14rpx;
		padding-left: 14rpx;
		padding-bottom: 14rpx;
		padding-right: 14rpx;
		position: absolute;
		top: 600px;
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
