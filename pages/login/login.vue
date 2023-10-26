<template>
	<view class="container container25505">
	
		<!-- <view class="flex flex-wrap diygw-col-24 flex-direction-column items-center flex-clz"> </view> -->
		<view class="flex flex-wrap diygw-col-24 flex-direction-column items-center flex-clz">
			<view class="flex flex-wrap diygw-col-24 flex-direction-column">
				<view class="flex diygw-col-24 justify-center">
					<view class="diygw-avatar xl radius bg-none">
						<image mode="aspectFit" class="diygw-avatar-img radius" src="/static/global/wode_unselect.png"></image>
					</view>
				</view>
			</view>
			<view class="flex diygw-col-24 flex-nowrap flex2-clz">
				<view class="diygw-col-8 text-clz diygw-text-md" > 手机号 </view>
				<input type="text" v-model="phone" placeholder="请输入手机号"/>
			</view>
			<view class="flex diygw-col-24 flex-nowrap flex3-clz">
				<view class="diygw-col-8 text2-clz diygw-text-md" > 密码 </view>
				<input type="safe-password" v-model="password" placeholder="请输入密码"/>
			</view>
			<view class="flex diygw-col-24">
				<button @tap="login" :disabled="disabled"
				style="background-color: #db5f54 !important" 
				class="diygw-btn green radius-xs flex-sub margin-xs 
				button-button-clz">登录</button>
			</view>
		</view>
		
		<view class="clearfix"></view>
	</view>
</template>

<script>
	import {
		request
	} from '@/utils/request';
	import {
		LOGIN_URL
	} from '@/utils/api';
import Login from '../../common/Login';
	
	export default {
		data() {
			return {
				//用户全局信息
				userInfo: {},
				//页面传参
				globalOption: {},
				//自定义全局变量
				globalData: {},
				inputFocus: false,
				phone:'13951905171',
				inputFocus: false,
				password:'123123'
			};
		},
		computed:{
			disabled(){
					if((this.phone ==='' || this.password ==='')){
						return true;
					}
					return false;
			}
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
		onReady(){
			this.$refs.formRef?.setRules(this.formRules);
		},
		methods: {
			async init() {
				await this.initResetform();
			},
			initResetform(){
				this.initform = JSON.stringify(this.form);
			},
			resetForm(){
				this.form = JSON.parse(this.initform);
			},
			async login(){
				console.log("BBBBBBBBBBBBBB")
				const loginDTO = {
					phone: this.phone,
					password: this.password
				}
				const res = await request(LOGIN_URL,'POST',loginDTO);
				console.log(res)
				if(res.success === true){
					uni.showToast({
						title:'登录成功'
					});
					console.log(JSON.stringify(res.data.user)+"&&&&&&&&&&&&")
					uni.setStorageSync('user',res.data.user);
					uni.setStorageSync('token',res.data.token);
					uni.switchTab({
						url:"/pages/mine/mine"
					})
				}
			}
		}
	};
</script>

<style lang="scss" scoped>
	.flex-clz {
		margin-left: 120rpx;
		padding-top: 10rpx;
		background-size: cover;
		padding-left: 10rpx;
		width: calc(100% - 120rpx - 130rpx) !important;
		padding-bottom: 10rpx;
		margin-top: 280rpx;
		background-image: url(/static/QQjt20231008145056.png);
		margin-bottom: 60rpx;
		height: 500rpx;
		margin-right: 130rpx;
		padding-right: 10rpx;
	}
	.flex2-clz {
		padding-top: 10rpx;
		padding-left: 10rpx;
		padding-bottom: 10rpx;
		border-bottom: 4rpx solid #eee;
		padding-right: 10rpx;
	}
	.text-clz {
		margin-left: 10rpx;
		padding-top: 10rpx;
		font-weight: bold;
		padding-left: 10rpx;
		width: calc(33.3333333333% - 10rpx - 10rpx) !important;
		padding-bottom: 10rpx;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		margin-right: 10rpx;
		padding-right: 10rpx;
	}
	.text1-clz {
		margin-left: 10rpx;
		padding-top: 10rpx;
		color: #8b8383;
		padding-left: 10rpx;
		width: calc(100% - 10rpx - 10rpx) !important;
		padding-bottom: 10rpx;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		margin-right: 10rpx;
		padding-right: 10rpx;
	}
	.flex3-clz {
		padding-top: 10rpx;
		padding-left: 10rpx;
		padding-bottom: 10rpx;
		border-bottom: 4rpx solid #eee;
		padding-right: 10rpx;
	}
	.text2-clz {
		margin-left: 10rpx;
		padding-top: 10rpx;
		font-weight: bold;
		padding-left: 10rpx;
		width: calc(33.3333333333% - 10rpx - 10rpx) !important;
		padding-bottom: 10rpx;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		margin-right: 10rpx;
		padding-right: 10rpx;
	}
	.text3-clz {
		margin-left: 10rpx;
		padding-top: 10rpx;
		color: #8b8383;
		padding-left: 10rpx;
		width: calc(100% - 10rpx - 10rpx) !important;
		padding-bottom: 10rpx;
		margin-top: 10rpx;
		margin-bottom: 10rpx;
		margin-right: 10rpx;
		padding-right: 10rpx;
	}
	.button-button-clz {
		margin: 3px !important;
	}
	.container25505 {
		padding-left: 0px;
		padding-right: 0px;
	}
	.container25505 {
	}
</style>
