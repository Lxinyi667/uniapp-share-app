<template>
<view class="diygw-list">
	<view  class="diygw-item col-100 row solid-bottom"  v-for="share in shares">
		<view class="diygw-avatar" style="position: relative;">
			<image mode="aspectFit" class="diygw-avatar-img" :src="share.cover"></image>
		</view>
		<view class="content">
			<view class="title" style="text-align: left;"> {{share.title}}</view>
			<view style="color: #aea3a1;text-align: left;height: 36px;overflow: hidden;" class="text-sm remark">{{share.summary}}</view>
		</view>
		<view class="sub" style="height: 100%;display: flex;align-items: center;">
			
			<text class="" style="font-size: 14px;">{{share.auditStatus}}</text>
			<text class="diy-icon-right" style="font-size: 18px;"></text>
		</view>
	</view>
	</view>
</template>

<script>
	import {MY_CONTRIBUTION_URL} from '@/utils/api.js';
	import {request } from '@/utils/request.js';
	export default {
		data() {
			return {
				shares:[],
				pageNo:1,
				pageSize:10,
				statusInfo:''
			}
		},
		onReachBottom() {
			console.log("下拉加载");
			if(!this.more){
				uni.showToast({
					title:'已加载完毕',
					duration:1000
				});
				return false;
			}
			this.pageNo = this.pageNo + 1;
			uni.showLoading({
				title:'加载中'
			});
			this.getShares(false);
			setTimeout(()=>{
				uni.hideLoading();
			},1000)
		},
		onPullDownRefresh() {
			this.getShares(true);
		},
		methods: {
			async getShares(init){
				if(init){
					this.pageNo = 1;
					this.more = true;
				}
				uni.showLoading({
					title:'加载中'
				});
				let res = await request(MY_CONTRIBUTION_URL,'GET',{
					pageNo:this.pageNo,
					pageSize:this.pageSize
				});
				console.log(res.data);
				setTimeout(()=>{
					uni.hideLoading();
				},100);
				if(init){
					this.shares = res.data;
					uni.stopPullDownRefresh();
					console.log("下拉刷新停止");
				}else{
					this.shares = this.shares.concat(res.data);
				}
				if(res.data.length < this.pageSize && this.pageNo > 0){
					this.more = false;
				}
				this.shares = this.shares.map(function(item){
					let res = {};
					switch(item.auditStatus){
						case 'NOT_YET':
							res.auditStatus = '尚未审核';
							break;
						case 'PASS':
							res.auditStatus = '审核通过';
							break;
						case 'REJECT':
							res.auditStatus = '审核被拒';
							break;
						default:
							res.auditStatus = item.auditStatus;
							break;
					}
					res.id = item.id;
					res.title = item.title;
					res.cover = item.cover;
					res.summary = item.summary;
					setTimeout(()=>{
						uni.hideLoading();
					},100);
					return res;					
				});
			}
			
		},
		onLoad() {
			this.getShares(true);
		}
	}
</script>

<style>

</style>
