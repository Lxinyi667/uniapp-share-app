<template>
	<view class="container container25502">
		<view class="diygw-text-line1 diygw-col-24 text-clz diygw-ellipsis diygw-text-md"> Node.js调试入门 </view>
		<view class="diygw-text-line1 diygw-col-24 text1-clz diygw-ellipsis diygw-text-sm"> 作者：Levis &nbsp;发布人：Infnityx7 &nbsp;积分：10 &nbsp;来源：原创 </view>
		<image src="/static/docker.jpg" class="response diygw-col-22 image-clz" mode="heightFix"></image>
		<view class="diygw-text-line1 diygw-col-24 text2-clz diygw-text-sm"> 学会高效调试 Node.js 会让日常开发更高效 </view>
		<view class="diygw-col-24 text3-clz diygw-text-sm"> 链接：https://pan.baidu.com/s/1Hk9i9VOTz2eSuy8p-kFGrq 密码：5njn </view>
		<u-form-item :borderBottom="false" class="diygw-col-24" label="通过审核" prop="switch">
			<view class="flex diygw-col-24 justify-end">
				<u-switch :activeValue="1" :inactiveValue="0" activeColor="#0081ff" @tap="changeSwitched" v-model="switched" slot="right"></u-switch>
			</view>
		</u-form-item>
		<u-form-item :borderBottom="false" class="diygw-col-24" label="立即发布" prop="switch1">
			<view class="flex diygw-col-24 justify-end">
				<u-switch :activeValue="1" :inactiveValue="0" activeColor="#0081ff" @tap="changeSwitch1" v-model="switch1" slot="right"></u-switch>
			</view>
		</u-form-item>
		<view class="flex diygw-col-24" style="padding: 0 8px 0 8px;">
			<button  class="diygw-btn red radius-xs flex-sub margin-xs button-button-clz"  @tap="sumbmit">提交</button>
		</view>
		<view class="clearfix"></view>
	</view>
</template>

<script>
		import{AUDIT_URL,SHARE_URL} from '../../utils/api.js';
		import{request} from '../../utils/request.js'
	export default {
		data() {
			return {
				//用户全局信息
				userInfo: {},
				//页面传参
				globalOption: {},
				//自定义全局变量
				globalData: {},
				switched: 1,
				switch1: 1,
				shareId:'',
								contribute: 1,
								pass: 1,
								share: null,
								nickname:'',
								avatarUrl:'',
								reason:''
			};
		},
		onShow() {
			this.setCurrentPage(this);
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
			changeSwitched(evt) {
							console.log(this.contribute)
						},
						changeSwitch1(evt) {
							console.log(this.pass)
						},
						async getShare(){
										let id=this.shareId;
										uni.showLoading({
											title:'加载中'
										});
										let res=await request(SHARE_URL+`/${id}`,'GET');
										setTimeout(()=>{
											uni.hideLoading();
										},1000);
										console.log(res.data);
										this.share=res.data.share;
										console.log(this.share);
										this.nickname=res.data.nickname
										this.avatarUrl=res.data.avatarUrl;
									},
									async sumbmit(){
										uni.showLoading({
											title:'审核中'
										});
										let res=await request(AUDIT_URL+`/${this.shareId}`,'POST',{
											auditStatusEnum: this.pass ? 'PASS':'REJECT',
											reason: this.pass ? '通过审核' : this.reason,
											showFlag: this.contribute
										});
										setTimeout(()=>{
											uni.hideLoading();
										},500);
										console.log(res.data);
										if(res.success){
											
											uni.showToast({
												title:'审核通过',
												duration:1000
											})
											uni.switchTab({
												url:'/pages/index/index'
											});
										}
									}
		}
	};
</script>

<style lang="scss" scoped>
	.text-clz {
		padding-top: 20rpx;
		font-weight: bold;
		padding-left: 20rpx;
		padding-bottom: 20rpx;
		padding-right: 20rpx;
	}
	.text1-clz {
		margin-left: 10rpx;
		padding-top: 10rpx;
		font-weight: bold;
		padding-left: 20rpx;
		width: calc(100% - 10rpx - 10rpx) !important;
		padding-bottom: 10rpx;
		margin-top: 0rpx;
		margin-bottom: 0rpx;
		margin-right: 10rpx;
		padding-right: 20rpx;
	}
	.image-clz {
		padding-top: 20rpx;
		padding-left: 20rpx;
		padding-bottom: 20rpx;
		padding-right: 20rpx;
	}
	.text2-clz {
		margin-left: 10rpx;
		padding-top: 10rpx;
		font-weight: bold;
		padding-left: 20rpx;
		width: calc(100% - 10rpx - 10rpx) !important;
		padding-bottom: 10rpx;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		margin-right: 10rpx;
		padding-right: 20rpx;
	}
	.text3-clz {
		margin-left: 10rpx;
		padding-top: 10rpx;
		font-weight: bold;
		padding-left: 20rpx;
		width: calc(100% - 10rpx - 10rpx) !important;
		padding-bottom: 10rpx;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		border-bottom: 2rpx solid #eee;
		margin-right: 10rpx;
		padding-right: 20rpx;
	}
	.button-button-clz {
		margin: 3px !important;
	}
	.container25502 {
		padding-left: 0px;
		padding-right: 0px;
	}
	.container25502 {
	}
</style>
