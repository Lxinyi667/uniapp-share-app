<template>
	<view class="container container25502">
		<view class="flex diygw-col-24" v-for="item in logs">
			<view class="diygw-list small not-remark">
				<view style="" class="diygw-item col-100 row arrow solid-bottom">
					<view style="color: #b0b0b0" class="diygw-avatar diy-icon-down"> </view>
					<view class="content">
						<view class="title"> {{item.createTime}}--{{item.description}} {{item.value}} </view>
					</view>
				</view>
			</view>
		</view>
		<view class="clearfix"></view>
	</view>
</template>

<script>
	import {request} from '@/utils/request.js';
	import {LOGS_URL} from '@/utils/api.js';
	export default {
		data() {
			return {
				//用户全局信息
				userInfo: {},
				pageNo:1,
				pageSize:20,
				more:true,
				logs:[]
			};
		},
		onShow() {
			this.getLogs(true);
		},
		onLoad(option) {
			this.userInfo = uni.getStorageSync('userInfo');
		},
		onPullDownRefresh() {
			this.getLogs(true);
		},
		methods: {
			async init() {},
			async getLogs(init){
				if(init){
					this.pageNo = 0;
					this.more = true;
				}
				let res = await request(LOGS_URL + '/' + this.userInfo.id + '/' + this.pageNo + '/' + this.pageSize,'GET');
				console.log(res);
				setTimeout(()=>{
					uni.hideLoading();
				},1000);
				
				if(init){
					this.logs = res.data;
					console.log(res.data);
					setTimeout(()=>{
						uni.stopPullDownRefresh();
					},1000)
					
				}else{
					this.logs = this.logs.concat(res.data)
				}
				
				if(res.data.length < this.pageSize && this.pageNo > 0){
					this.more = false;
				}
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
			this.getLogs(false);
			setTimeout(()=>{
				uni.hideLoading();
			},1000)
		}
	};
</script>

<style lang="scss" scoped>
	.container25502 {
		padding-left: 0px;
		padding-right: 0px;
	}
	.container25502 {
	}
</style>
