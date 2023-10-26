<template>
	<view class="container container25487">
		<view class="flex diygw-col-24 flex-direction-column">
			<view class="diygw-tabs text-center solid-bottom justify-center tabs-title">
				<view class="diygw-tab-item tabs-item-title flex-sub" :class="index == tabsIndex ? ' cur text-green ' : ''" v-for="(item, index) in tabsDatas" :key="index" @click="changeTabs" :data-index="index"> <text v-if="item.icon" :class="item.icon"></text> {{ item.text }} </view>
			</view>
			<view class="">
				<view v-if="tabsIndex == 0" class="flex-sub">
					<view class="flex diygw-col-24">
						<diy-noticebar class="flex1 diy-notice-bar" color="#ec544e" bgColor="#fff" leftIcon="diy-icon-notification">
							<block v-slot:content>
								<text class="diy-notice-item"> {{ message }} </text>
							</block>
						</diy-noticebar>
					</view>
					<view class="diygw-col-24">
						<view class="diygw-search">
							<view class="flex1 align-center flex padding-xs solid radius search-search" style="width: 700rpx">
								<text style="color: #555 !important" class="diy-icon-search"></text>
								<input class="flex1" name="search" type="" v-model="search" placeholder="请输入关键字" />
							</view>
						</view>
					</view>
					
					<view v-for="(share,index) in shares" :key="index">
						<view class="flex diygw-col-24 flex-nowrap flex-clz">
							<view class="flex diygw-col-6">
								<view class="diygw-avatar radius bg-none" style="position: relative;">
									<image mode="aspectFit" class="diygw-avatar-img" 
										   :src="share.cover"></image>
									<div v-if="share.isOriginal" 
									style="position: absolute; top: 0; left: 0; height: 18px; width: 30px; 
									background-color: red; font-size: 12px; text-align: center;">转载</div>
									<div  v-else 
									style="position: absolute; top: 0; left: 0; height: 18px; width: 30px; 
									background-color: green; font-size: 12px; text-align: center;">原创</div>
								</view>
							</view>
							<view class="flex flex-wrap diygw-col-24 flex-direction-column">
								<view class="diygw-col-24 text-clz"> {{share.title}} </view>
								<view class="diygw-text-line2 diygw-col-24"> {{share.summary}} </view>
							</view>
							<view class="flex flex-wrap diygw-col-6 flex-direction-column justify-center flex2-clz">
								<view class="diygw-col-24 text2-clz"> {{share.price}} 积分 </view>
								<view  class="diygw-col-24"  @tap="gotoDetail(share.id)" 
								v-if="share.downloadUrl"> 下载 </view>		
								<!-- <view  class="diygw-col-24"  @tap="navigateTo" data-type="page" data-url="/pages/xiangqin/xiangqin" 
								v-if="share.downloadUrl"> 下载 </view>		 -->						
								<!-- <view @tap="navigateTo" data-type="page" data-url="/pages/xiangqin/xiangqin" 
								class="diygw-text-line2 diygw-col-23 text3-clz"> 下载 </view> -->
								<view class="diygw-col-24 padding-top-xs" v-else @tap="gotoExchange(share.id)" > 兑换 </view>
							</view>
						</view>
					</view>
					
					
				</view>

				<view v-if="tabsIndex == 1" class="flex-sub">
					
						<view class="flex diygw-col-23 flex-nowrap">
							<view class="flex diygw-col-6 justify-center">
								<view class="diygw-avatar radius bg-none">
									<image mode="aspectFit" class="diygw-avatar-img radius size-small" src="/static/grid1.png"></image>
								</view>
							</view>
							<view class="diygw-col-13 text12-clz  padding-top-sm" > <text class="red padding-bottom-xs padding-left-sm padding-right-sm padding-top-xs">01 积分获取方式 </text></view>
						
						</view>
						<view class="flex flex-wrap diygw-col-24 flex-direction-column padding-left-lg padding-bottom-sm">
							<view class="diygw-col-6 text13-clz"> 每日签到 </view>
							<view class="diygw-col-24 text14-clz"> 投稿 </view>
						</view>
					
					
					<!-- 2 -->
					<view class="flex diygw-col-23 flex-nowrap">
						<view class="flex diygw-col-6 justify-center">
							<view class="diygw-avatar radius bg-none">
								<image mode="aspectFit" class="diygw-avatar-img radius size-small" src="/static/grid1.png"></image>
							</view>
						</view>
						<view class="diygw-col-13 text12-clz   padding-top-sm "><text class="red padding-bottom-xs padding-left-sm padding-right-sm padding-top-xs"> 02 深入交流</text> </view>
					
					</view>
					<view class="flex flex-wrap diygw-col-24 flex-direction-column padding-left-lg  padding-bottom-sm">
						<view class="diygw-col-6 text13-clz "> 技术交流群QQ群：88888888 </view>
						<view class="diygw-col-24 text14-clz"> 私人微信：InfinityX7 </view>
					</view>
					<!-- 3 -->
					<view class="flex diygw-col-23 flex-nowrap">
						<view class="flex diygw-col-6 justify-center">
							<view class="diygw-avatar radius bg-none">
								<image mode="aspectFit" class="diygw-avatar-img radius size-small" src="/static/grid1.png"></image>
							</view>
						</view>
						<view class="diygw-col-13 text12-clz   padding-top-sm"> <text class="red padding-bottom-xs padding-left-sm padding-right-sm padding-top-xs">03 公众号（技术干货分享）</text> </view>
					
					</view>
					<view class="flex flex-wrap diygw-col-24 flex-direction-column padding-left-lg padding-bottom-sm">
						<view class="diygw-col-12 text13-clz"> 点击右上角->松山院->公众号</view>
						<view class="diygw-col-24 text14-clz"> 公众号微信：ssy </view>
					</view>
				</view>
			</view>
		</view>
		
		<view class="clearfix"></view>
	</view>
</template>

<script>
	import {request} from '@/utils/request.js';
	import {LATEST_NOTICE_URL,SHARE_LIST_URL} from '@/utils/api.js';
	export default {
		data() {
			return {
				message: "",
				//用户全局信息
				userInfo: {},
				//页面传参
				globalOption: {},
				//自定义全局变量
				globalData: {},
				tabsDatas: [
					{ text: `发现`, icon: `` },
					{ text: `使用说明`, icon: `` }
				],
				tabsLeft: 0,
				tabsWidth: 0,
				tabsItemWidth: 0,
				tabsIndex: 0,
				search: '',
				notice:'',
				shares: [],
				pageNo: 1,
				pageSize: 8,
				more: true
			};
		},
		onShow() {
			this.setCurrentPage(this);
			this.getNoticeList();
			this.getShares(true);
		},
		onLoad(option) {
			this.setCurrentPage(this);
			if (option) {
				this.setData({
					globalOption: this.getOption(option)
				});
			}

			this.getShares();
		},
		//下拉刷新
		onPullDownRefresh() {
			//参数为true ，就是重新开始加载
			this.getShares(true);
		},
		//触底加载更多
		onReachBottom() {
			//没有更多数据了
			if(!this.more){
				uni.showToast({
					title:'已加载完毕',
					duration:1000
				});
				//直接返回，不执行下面的代码了
				return false;
			}
			//正常加载下一页
			this.pageNo=this.pageNo+1;
			uni.showLoading({
				title:'加载中'
			});
			//不带参数的请求,就是分页加载
			this.getShares();
			setTimeout(()=>{
				uni.hideLoading();
			},1000);
		},
		
		methods: {	
			async init() {},
			gotoDetail(id){
				uni.navigateTo({
					url:"/pages/xiangqin/xiangqin?id=" + id
				})
			},
			
			gotoExchange(id){
				uni.navigateTo({
					url:"/pages/toexchange/toexchange?id=" + id
				})
			},
			
			async getNoticeList(){
				const res=await request(LATEST_NOTICE_URL,"GET");
				this.message=res.data.content
				console.log(res);
			},
			async getShares(init){
				//入参为ture 从第一页重新开始加载数据，场景为：下拉刷新，每次进入
				if(init){
					this.pageNo=1;
					this.more=true;
				}
				
				let res =await request(SHARE_LIST_URL,'GET',{
					pageNo:this.pageNo,
					pageSize:this.pageSize
				});
				
				//请求结束时隐藏动画
				setTimeout(()=>{
					uni.hideLoading();
				},2000);
				
				//处理进入首页加载数据和下拉刷新场景
				if(init){
					//获得新的数据集合，将数据的初始化为本次请求返回结果
					this.shares=res.data;
					//停止下拉刷新
					uni.stopPullDownRefresh();
				}else{
					//不是下拉刷新，是加载下一页数据，则把新数据追加到后面，不能覆盖原值
					this.shares=this.shares.concat(res.data);
				}
				//加载到最后一页
				if(res.data.length<this.pageSize&&this.pageNo>0){
					this.more=false;
				}
				// console.log(res);
				// this.shares=res.data;
			},
			changeTabs(evt) {
				let { index } = evt.currentTarget.dataset;
				if (index == this.tabsIndex) return;
				this.setData({
					tabsIndex: index
				});
			}
		}
	};
</script>

<style lang="scss" scoped>
	.tabs-item-title.cur {
		color: #ec544e !important;
	}
	.flex-clz {
		padding-top: 10rpx;
		padding-left: 10rpx;
		padding-bottom: 10rpx;
		border-bottom: 2rpx solid #eee;
		padding-right: 10rpx;
	}
	.text-clz {
		font-weight: bold;
		font-size: 28rpx !important;
	}
	.flex2-clz {
		margin-left: 10rpx;
		width: calc(25% - 10rpx - 10rpx) !important;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		margin-right: 10rpx;
	}
	.text2-clz {
		font-weight: bold;
	}
	.flex3-clz {
		padding-top: 10rpx;
		padding-left: 10rpx;
		padding-bottom: 10rpx;
		border-bottom: 2rpx solid #eee;
		padding-right: 10rpx;
	}
	.text4-clz {
		font-weight: bold;
		font-size: 28rpx !important;
	}
	.flex5-clz {
		margin-left: 10rpx;
		width: calc(25% - 10rpx - 10rpx) !important;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		margin-right: 10rpx;
	}
	.text6-clz {
		font-weight: bold;
	}
	.flex6-clz {
		padding-top: 10rpx;
		padding-left: 10rpx;
		padding-bottom: 10rpx;
		padding-right: 10rpx;
	}
	.flex7-clz {
		padding-top: 10rpx;
		padding-left: 10rpx;
		padding-bottom: 10rpx;
		padding-right: 10rpx;
	}
	.flex8-clz {
		background-color: #ec544e;
		padding-top: 16rpx;
		color: #ffffff;
		font-weight: bold;
		padding-left: 60rpx;
		font-size: 28rpx !important;
		padding-bottom: 16rpx;
		padding-right: 60rpx;
	}
	.text8-clz {
		padding-top: 16rpx;
		border-right: 6rpx solid #ffffff;
		color: #ffffff;
		font-weight: bold;
		padding-left: 16rpx;
		font-size: 28rpx !important;
		padding-bottom: 16rpx;
		border-bottom: 6rpx solid #ffffff;
		background-color: #ec544e;
		flex-shrink: 0;
		top: 0rpx;
		left: 0rpx;
		width: 12rpx !important;
		position: absolute;
		height: 12rpx !important;
		padding-right: 16rpx;
	}
	.flex10-clz {
		background-color: rgba(192, 164, 164, 0.16);
		padding-top: 10rpx;
		padding-left: 10rpx;
		padding-bottom: 10rpx;
		padding-right: 10rpx;
	}
	.text12-clz {
		margin-left: 10rpx;
		width: calc(100% - 10rpx - 10rpx) !important;
		font-size: 28rpx !important;
		margin-top: 0rpx;
		margin-bottom: 10rpx;
		margin-right: 10rpx;
	}
	.text13-clz {
		margin-left: 10rpx;
		width: calc(100% - 10rpx - 10rpx) !important;
		font-size: 28rpx !important;
		margin-top: 0rpx;
		margin-bottom: 10rpx;
		margin-right: 10rpx;
	}
	.flex14-clz {
		padding-top: 10rpx;
		padding-left: 10rpx;
		padding-bottom: 10rpx;
		padding-right: 10rpx;
	}
	.flex15-clz {
		padding-top: 10rpx;
		padding-left: 10rpx;
		padding-bottom: 10rpx;
		padding-right: 10rpx;
	}
	.flex16-clz {
		background-color: #ec544e;
		padding-top: 16rpx;
		color: #ffffff;
		font-weight: bold;
		padding-left: 60rpx;
		font-size: 28rpx !important;
		padding-bottom: 16rpx;
		padding-right: 60rpx;
	}
	.text16-clz {
		padding-top: 16rpx;
		border-right: 6rpx solid #ffffff;
		color: #ffffff;
		font-weight: bold;
		padding-left: 16rpx;
		font-size: 28rpx !important;
		padding-bottom: 16rpx;
		border-bottom: 6rpx solid #ffffff;
		background-color: #ec544e;
		flex-shrink: 0;
		top: 0rpx;
		left: 0rpx;
		width: 12rpx !important;
		position: absolute;
		height: 12rpx !important;
		padding-right: 16rpx;
	}
	.flex17-clz {
		background-color: rgba(192, 164, 164, 0.16);
		padding-top: 10rpx;
		padding-left: 10rpx;
		padding-bottom: 10rpx;
		padding-right: 10rpx;
	}
	.text18-clz {
		margin-left: 10rpx;
		width: calc(100% - 10rpx - 10rpx) !important;
		font-size: 28rpx !important;
		margin-top: 0rpx;
		margin-bottom: 10rpx;
		margin-right: 10rpx;
	}
	.text19-clz {
		margin-left: 10rpx;
		width: calc(100% - 10rpx - 10rpx) !important;
		font-size: 28rpx !important;
		margin-top: 0rpx;
		margin-bottom: 10rpx;
		margin-right: 10rpx;
	}
	.flex9-clz {
		padding-top: 10rpx;
		padding-left: 10rpx;
		padding-bottom: 10rpx;
		padding-right: 10rpx;
	}
	.flex11-clz {
		padding-top: 10rpx;
		padding-left: 10rpx;
		padding-bottom: 10rpx;
		padding-right: 10rpx;
	}
	.flex12-clz {
		background-color: #ec544e;
		padding-top: 16rpx;
		color: #ffffff;
		font-weight: bold;
		padding-left: 60rpx;
		font-size: 28rpx !important;
		padding-bottom: 16rpx;
		padding-right: 60rpx;
	}
	.text10-clz {
		padding-top: 16rpx;
		border-right: 6rpx solid #ffffff;
		color: #ffffff;
		font-weight: bold;
		padding-left: 16rpx;
		font-size: 28rpx !important;
		padding-bottom: 16rpx;
		border-bottom: 6rpx solid #ffffff;
		background-color: #ec544e;
		flex-shrink: 0;
		top: 0rpx;
		left: 0rpx;
		width: 12rpx !important;
		position: absolute;
		height: 12rpx !important;
		padding-right: 16rpx;
	}
	.flex13-clz {
		background-color: rgba(192, 164, 164, 0.16);
		padding-top: 10rpx;
		padding-left: 10rpx;
		padding-bottom: 10rpx;
		padding-right: 10rpx;
	}
	.text14-clz {
		margin-left: 10rpx;
		width: calc(100% - 10rpx - 10rpx) !important;
		font-size: 28rpx !important;
		margin-top: 0rpx;
		margin-bottom: 10rpx;
		margin-right: 10rpx;
	}
	.text15-clz {
		margin-left: 10rpx;
		width: calc(100% - 10rpx - 10rpx) !important;
		font-size: 28rpx !important;
		margin-top: 0rpx;
		margin-bottom: 10rpx;
		margin-right: 10rpx;
	}
	.container25487 {
		padding-left: 0px;
		padding-right: 0px;
	}
	.container25487 {
	}
</style>
