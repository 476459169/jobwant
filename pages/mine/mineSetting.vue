<template>
	<view class="">

		<view class="line" style="position: fixed;top: 0px;"></view>
		<view class="topView flex_view">
			<image class="top_image" :src="picStr.length>0?picStr:userInfo.imgUrl?baseUrl+userInfo.imgUrl:'../../static/login/loginTX.png' "
			 mode=""></image>
			<view class="change" @click="changePic()">
				更换头像 ⟩
			</view>
		</view>

		<view class="flex_view itemView">
			<image src="../../static/image/mine/detailAdr@3x.png" mode=""></image>
			<view class="itemView_title">您的昵称</view>
			<input class="itemView_content" type="text" v-model="userName" />
		</view>
		<!-- <view class="line"></view> -->

		<!-- <view class="flex_view itemView">
			<image src="../../static/image/mine/cerPerson@3x.png" mode=""></image>
			<view class="itemView_title">真实姓名</view>
			<input class="itemView_content" type="text" v-model="userRealName" />
		</view> -->
		<!-- <view class="line"></view> -->


		<view class="flex_view itemView">
			<image src="../../static/image/mine/phone@2x.png" mode=""></image>
			<view class="itemView_title">手机号</view>
			<view class="itemView_content" >{{userInfo.tel}} ⟩</view>
		</view>
		<!-- <view class="line"></view> -->
		<!-- <view class="flex_view itemView">
			<image src="../../static/image/mine/cerPerson@3x.png" mode=""></image>
			<view class="itemView_title">个人资料</view>
			<view class="itemView_content">完成度 50% ⟩</view>
		</view> -->
		<!-- <view class="line"></view> -->


		<view class="bottomBtn" @click="upNewUserMsg()">
			保存更改
		</view>
		
		<view class="bottomBtn_logout" @click="loginOut()">
			退出登录
		</view>
	</view>
</template>

<script>
	var _this;
	export default {
		data() {
			return {
				userInfo: Object,
				baseUrl: "http://39.105.48.243:8080/crlink/",
				userName: '',
				userRealName: '',
				picStr: '',
				sfz: ''

			}
		},
		onLoad() {
			_this = this;
			this.baseUrl = getApp().globalData.baseUrl
			this.getuserInfo()
		},
		onShow() {
			this.getuserInfo()
		},
		methods: {
			getuserInfo() {
				var that = this;
				var loginkey = uni.getStorageSync('loginKey');
				this.$api.post('user!ajaxGetUserInfo.action', {
					loginKey: loginkey
				}).then(res => {
					if (res.res.status == 0) {
						that.userInfo = res.inf
						that.userName = that.userInfo.nickName
						that.userRealName = that.userInfo.realName
						that.sfz = that.userInfo.subCardNo
					} else {
						uni.showToast({
							title: res.res.error
						})
					}
				})
			},

			telClcik() {
				// uni.navigateTo({
				// 	url:'./changePhoneNumber'
				// })
				uni.navigateTo({
					url: './changePhoneNumber'
				})
			},
			changePic() {
				// 用户点击了从图库上传
				console.log("用户点击了从图库上传");
				uni.chooseImage({
					count: 1,
					sizeType: ["compressed"],
					success(response) {



						// 选择图片后, 返回的数据
						var fileUrl = response.tempFilePaths[0]
						_this.picStr = fileUrl
						console.log("用户点击了从图库上传222 =" + fileUrl);
					}
				})
			},

			upNewUserMsg() {
				console.log("111111+_this.picStr=" + this.picStr);
				var loginkey = uni.getStorageSync('loginKey');
				if (_this.picStr) {
					if (_this.picStr.length > 0) {
						uni.uploadFile({
							url: this.baseUrl + 'app/qzUser!ajaxUpdateQZUserInfo.action', //仅为示例，非真实的接口地址
							filePath: _this.picStr ? _this.picStr : _this.userInfo.imgUrl ? _this.baseUrl + _this.userInfo.imgUrl : '../../static/image/mine/default_head_img@2x.png',
							name: 'headerImgFile',
							formData: {
								'loginKey': loginkey,
								'realName': this.userRealName,
								'nickName': this.userName,
								'subCardNo': this.sfz
							},
							success: (uploadFileRes) => {
								uni.showToast({
									title: "更改成功"
								})
							}

						})
					}
				} else {
					var loginkey = uni.getStorageSync('loginKey');
					this.$api.post('qzUser!ajaxUpdateQZUserInfo.action', {
						loginKey: loginkey,
						realName: this.userRealName,
						sex: "1",
						nickName: this.userName,
						subCardNo: this.sfz

					}).then(res => {
						if (res.res.status == 0) {
							uni.showToast({
								title: "更改成功"
							})
						} else {
							uni.showToast({
								title: res.res.error
							})
						}
					})
				}
			},
			
			loginOut(){
				uni.removeStorageSync('loginKey');
				uni.removeStorageSync('userId');
				uni.removeStorageSync('isFill');
				uni.navigateBack({
					delta:1
				})
			}
			
		},
		computed: {

		}
	}
</script>

<style lang="scss">
	.topView {
		margin-top: 1px;
		width: 100%;
		background-color: #FFFFFF;

		.top_image {
			// padding: 15px;
			margin: 15px;
			width: 180upx;
			height: 180upx;
			border-radius: 90upx;
		}

		.change {
			flex: 1;
			display: flex;
			justify-content: flex-end;
			margin-right: 15px;
			color: #666666;
			font-size: 14px;
		}

	}

	.itemView {
		padding: 15px 15px 5px 15px;
		background-color: #FFFFFF;

		image {
			height: 15px;
			width: 13px;
		}

		.itemView_title {
			color: #666666;
			margin-left: 5px;
			font-size: 14px;
		}

		.itemView_content {
			color: #666666;
			flex: 1;
			display: flex;
			justify-content: flex-end;
			margin-right: 5px;
			font-size: 14px;
			text-align: right;
		}
	}

	.flex_view {
		display: flex;
		align-items: center;
	}

	.line {
		width: calc(100vw - 30px);
		margin-left: 15px;
		height: 1px;
		background-color: #e6e6e6;
	}

	.bottomBtn {
		position: fixed;
		bottom: 60px;
		width: calc(100vw - 30px);
		left: 15px;
		height: 40px;
		color: #FFFFFF;
		background-color: #e8654b;
		font-size: 15px;
		line-height: 40px;
		text-align: center;
	}
	
	.bottomBtn_logout {
		position: fixed;
		bottom: 10px;
		width: calc(100vw - 30px);
		left: 15px;
		height: 40px;
		color: #e8654b;
		background-color: #FFFFFF;
		font-size: 15px;
		line-height: 40px;
		text-align: center;
		border: 1px solid #f5f6f8;
	}
</style>
