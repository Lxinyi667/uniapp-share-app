<template>
	<view class="container container25493">
		<view class="diygw-text-line3 diygw-col-24 text-clz"> 说明：投稿审核通过后会有积分奖励；资源被下载会有积分奖励；提交的资源不得包含广告、侵权信息、百度网盘地址建议有密码。 </view>
		<view class="flex flex-wrap diygw-col-24 flex-direction-column">
			<u-form :model="form" :rules="formRules" :errorType="['message', 'toast']" ref="formRef" class="flex diygw-form diygw-col-24">
				<u-form-item :borderBottom="false" class="diygw-col-24" labelPosition="top" prop="isOriginal">
					<u-radio-group class="flex flex-wrap diygw-col-24 justify-around" wrapClass=" justify-around" activeColor="#1cbbb4" v-model="form.isOriginal" @change="changeFormRadio">
						<u-radio shape="circle" v-for="(radioitem, radioindex) in formData.radioDatas" :key="radioindex" :name="radioitem.value" :checked="radioitem.checked">
							{{ radioitem.label }}
						</u-radio>
					</u-radio-group>
				</u-form-item>
				<u-form-item class="diygw-col-24" label="标题" prop="title">
					<u-input :focus="formData.inputFocus" class="" placeholder="请输入标题" v-model="form.title" type="text"></u-input>
				</u-form-item>
				<u-form-item class="diygw-col-24" label="作者" prop="author">
					<u-input :focus="formData.input2Focus" class="" placeholder="请输入作者" v-model="form.author" type="text"></u-input>
				</u-form-item>
				<u-form-item class="diygw-col-24" label="价格" prop="price">
					<u-input :focus="formData.input1Focus" class="" placeholder="请输入价格" v-model="form.price" type="text"></u-input>
				</u-form-item>
				<u-form-item class="diygw-col-24" label="简介" prop="summary">
					<u-input :focus="formData.input3Focus" class="" placeholder="介绍一下技术干货吧" v-model="form.summary" type="text"></u-input>
				</u-form-item>
				<u-form-item class="diygw-col-24" label="封面图" prop="cover">
					<u-input :focus="formData.input4Focus" class="" placeholder="https://img2.sycdn.image" v-model="form.cover" type="text"></u-input>
				</u-form-item>
				<u-form-item class="diygw-col-24" label="下载地址" prop="downloadUrl">
					<u-input :focus="formData.input5Focus" class="" placeholder="链接：https://pan.baidu.com" v-model="form.downloadUrl" type="text"></u-input>
				</u-form-item>
				<view class="flex diygw-col-24">
					<button class="diygw-btn red radius-xs flex-sub margin-xs button-button-clz" @tap="submitForm">提交</button>
				</view>
			</u-form>
		</view>
		<view class="clearfix"></view>
	</view>
</template>

<script>
	import {request} from '@/utils/request.js';
	import {CONTRIBUTE_URL } from '@/utils/api.js';
	export default {
		data() {
			return {
				//用户全局信息
				userInfo: {},
				//页面传参
				globalOption: {},
				//自定义全局变量
				globalData: {},
				form: {
					isOriginal: 1,
					title: '',
					author: '',
					price: '',
					summary: '',
					cover: '',
					downloadUrl: '',
					userId:''
				},
				formRules: {},
				formData: {
					radioDatas: [
						{ value: 1, label: '原创', checked: true },
						{ value: 0, label: '转载', checked: false }
					],
					inputFocus: false,
					input2Focus: false,
					input1Focus: false,
					input3Focus: false,
					input4Focus: false,
					input5Focus: false
				}
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
		onReady() {
			this.$refs.formRef?.setRules(this.formRules);
		},
		methods: {
			async init() {
				await this.initResetform();
			},
			changeFormRadio(evt) {},
			initResetform() {
				this.initform = JSON.stringify(this.form);
			},
			resetForm() {
				this.form = JSON.parse(this.initform);
			},

			async submitForm(e) {
				this.form.userId = uni.getStorageSync('user').id;
				if(!this.form.userId){
					uni.showToast({
						title:"请先登录再投稿！",
						icon:'error'
					})
				}else{
					const res = await request(CONTRIBUTE_URL,'POST',this.form);
					console.log(res);
					if(res==1){
						uni.showModal({
							title:"投稿成功，是否继续投稿？",
							success(res) {
								if(res.confirm){
								this.form = {
									isOriginal: 'true',
									title: '',
									author: '',
									price: '',
									summary: '',
									cover: '',
									downloadUrl: ''
								};
								uni.reLaunch({
									url:'/pages/contribute/contribute'
								})
								};
								if(res.cancel){
									uni.switchTab({
										url:'/pages/index'
									})
								}
							}
						})
					}else{
						uni.showToast({
							title:"投稿失败，" + res.message,
							icon:'error'
						})
					}
				}

			}
		}
	};
</script>

<style lang="scss" scoped>
	.text-clz {
		margin-left: 10rpx;
		width: calc(100% - 10rpx - 10rpx) !important;
		font-size: 30rpx !important;
		margin-top: 30rpx;
		margin-bottom: 10rpx;
		margin-right: 10rpx;
	}
	.button-button-clz {
		margin: 3px !important;
	}
	.container25493 {
		padding-left: 0px;
		padding-right: 0px;
	}
	.container25493 {
	}
</style>
