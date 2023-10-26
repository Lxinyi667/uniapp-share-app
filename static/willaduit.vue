<template>
<view class="diygw-list">
<view style="" class="diygw-item col-100 row solid-bottom">
	<view class="diygw-avatar" style="position: relative;">
		<image mode="aspectFit" class="diygw-avatar-img" src="@/static/react.png"></image>
	</view>
	<view class="content">
		<view class="title" style="text-align: left;"> React技术核心</view>
		<view style="color: #aea3a1;text-align: left;height: 38px;overflow: hidden;" class="text-sm remark">介绍内容介绍内容介绍内容介绍内容介绍内容介绍内容介绍内容 </view>
	</view>
	<view class="sub" style="color: #aea3a1; display: flex;height: 100%;line-height: 50px;" @tap="jumpToAudit">
		去审核 <text class="diy-icon-right" style="color:#aea3a1;font-size: 18px;"></text>
	</view>
</view>
</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			jumpToAudit(){
				uni.navigateTo({
					url:'/pages/audit/audit'
				})
			}
		}
	}
</script>

<style>

</style>
