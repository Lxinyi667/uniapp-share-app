<template>
<view class="diygw-list">
<view style="" class="diygw-item col-100 row solid-bottom" v-for="item in shares">
	<view class="diygw-avatar" style="position: relative;">
		<image mode="aspectFit" class="diygw-avatar-img" :src="item.cover" style="border-radius: 50%;"></image>
		<view v-if="item.isOriginal" class="tag" style="position: absolute;height: 16px;font-size: 10px; height: fit-content;width: fit-content;background-color: #db5e54;color: white;padding: 2px 5px 2px 5px;left: -7px;top: -5px;">
			原创
		</view>
		<view v-else class="tag" style="position: absolute;height: 16px;font-size: 10px; height: fit-content;width: fit-content;background-color: #008001;color: white;padding: 2px 5px 2px 5px;left: -7px;top: -5px;">
			转载
		</view>
	</view>
	<view class="content">
		<view class="title" style="text-align: left;">{{item.title}} </view>
		<view style="color: #aea3a1;text-align: left;height: 38px;overflow: hidden;" class="text-sm remark">{{item.summary}}</view>
	</view>
	<view class="sub" style="text-align: right;">
		<text>{{item.price}}积分</text><br>
		<text  @click="jumpToDownload(item.id)">下载</text>
	</view>
</view>
</view>
</template>

<script>
	import {MY_EXCHANGE} from '@/utils/api.js';
	import {request} from '@/utils/request.js';
	export default {
		data() {
			return {
				id:'',
				shares:[]
			}
		},
		onShow() {
			this.id = uni.getStorageSync('userInfo').id;
			this.getShares(this.id);
		},
		methods: {
			async getShares(id){
			  const res = await request(MY_EXCHANGE + '/' + id,"GET");
			  console.log(res.data);
			  this.shares = res.data;
			},
			jumpToDownload(id){
				uni.navigateTo({
					url:'/pages/detail_d/detail_d?id=' + id
				})
			}
		}
	}
</script>

<style>

</style>
