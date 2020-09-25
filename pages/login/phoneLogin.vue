<template>
	<view class="bd">
		
		
		
		
		
		<view style="margin-top: 100upx;">
			
			
			<view class="header_title">
				<text style="color: #F56C6C;">hi</text>&nbsp欢迎来到临研招聘
			</view>
			<view class="header_title_s">
				首次登录将自动注册
			</view>
			
			<view class="title">
				手机号
			</view>
			<view class="item">
				<input class="item_ct" type="text" value="" placeholder="请输入手机号" maxlength="11" v-model="phoneNum" />
				<button class="yz"  @click="getVerificationCode" :disabled="disabled">{{VerificationMes}} </button>
			</view>
			
			<view class="title">
				验证码
			</view>
			<view class="item yzm_w">
				<input class="item_ct" type="text" value=""  maxlength="11" v-model="VerificationCode" />
			</view>
			
			<view style="margin-top: 100upx;"> 
				<button class="logbtn" @click="login()">登 录</button>
			</view>

		<!-- <view style="margin-top: 20upx;">
			<view class="registbtn" v-on:click="jumpToRegist">新用户？点击这里注册</view>
		</view> -->

	</view>


	</view>
</template>

<script>
	var self;
	export default {

		data() {
			return {
				phoneNum: null,
				VerificationCode: null,
				VerificationMes: "发送验证码",
				disabled: false,
				loginInfo: Object,
				scope: 0
			}
		},

		onShow() {
				
		},

		onLoad() {
			self = this;
		},
		methods: {


			getVerificationCode() {
				if (!this.phoneNum || this.phoneNum.length !== 11) {
					uni.showToast({
						title: '请检查手机号！'
					});
				} else {
					var ii = 0;
					this.disabled = true;
					this.VerificationMes = "60s";
					this.timer = setInterval(() => {

						ii = ii + 1;
						console.log("ii=" + ii);
						this.VerificationMes = (60 - ii).toString() + "s";

						if (ii == 30) {
							this.disabled = false;
							this.VerificationMes = "获取验证码";
							ii = 0;
							clearInterval(this.timer);
						}
					}, 1000)

					this.$api.post('login!ajaxGetPhoneCode.action', {
						tel: this.phoneNum
					}).then(res => {
						if (res.res.status == 0) {
							console.log("获取验证码成功")
						}
					})

				}

			},


			login() {

				if (this.phoneNum.length == 11 && this.VerificationCode.length > 0) {
					// console.log("手机号"+this.phoneNum+"yzm"+this.VerificationCode)
					//注册成功 调用登录接口
					this.$api.post('login!ajaxLogin.action', {
						tel: this.phoneNum,
						phoneCode: this.VerificationCode
					}).then(res => {
						if (res.res.status == 0) {
							var dict = res.inf;
							console.log(dict);
							uni.setStorageSync('userId', dict.userId);
							uni.setStorage({
								key: "loginKey",
								data: dict.loginKey,
								success() {
									uni.showToast({
										title: '登录中···'
									});
									setTimeout(function() {
										uni.navigateBack({
											delta: 1
										})
									}, 1500)
								}
							})
						}
					})

				} else {
					uni.showToast({
						title: '请检查填写项！'
					});
				}

			}

		}
	}
</script>

<style lang="scss">
	.bd {
		margin-top: 0upx;
		width: 100%;
		height: 100%;
	}

	.item {
		display: flex;
		height: 35px;
		margin: 0px 10px;
		margin-top: 10px;
		box-sizing: border-box;
		background-color: #f5f7f8;
		border-radius: 5px;
		
		image {
			padding-left: 40upx;
			// padding-top: 40upx;
			height: 40upx;
			width: 30upx;
		}

		.item_ct {
			flex: 1;
			height: 35px;
			margin-left: 10px;
			line-height: 35px;
			font-size: 28upx;
			color: #666666;
			

		}
 
		.yz {
			color: #fd6666;
			font-size: 24upx;
			padding-right: 5%;
			border: 1px solid #FFFFFF;
			background: #FFFFFF;
			;
		}
		

	}

		
	.yzm_w{
		width: 120px;
	}


	.line {
		width: 90%;
		height: 1upx;
		margin-left: 5%;
		background-color: #e6e6e6;
	}

	.logbtn {
		background-color: #ee7b5e;
		color: #FFFFFF;
		font-size: 30upx;
		// padding: 0px 0upx;
		width: 60%;
		margin-left: 20%;
		text-align: center;
		font-weight: bold;

	}
	
		
	.title{
		margin-left: 20px;
		line-height: 20px;
		height: 20px;
		font-size: 14px;
		margin-top: 20px;
	}


	.registbtn {
		background-color: transparent;
		color: #ee7b5e;
		font-size: 30upx;
		padding:5px 0upx;
		width: 90%;
		margin-left: 5%;
		text-align: center;
		font-weight: bold;
		border-color: #666666;
		border-width: 1upx;
	}
	
	.header_title{
		margin: 50px 10px 10px 10px;
		height: 20px;
		line-height: 20px;
		font-size: 16px;
	}
	
		
	.header_title_s{
		margin: 0px 100px 20px 10px;
		height: 20px;
		line-height: 20px;
		font-size: 14px;
		color: #666666;
	}
	
</style>
