<template>
	<view>
		<view class="itemview" style="margin-top: 10px;">
			<view class="itemview_title">证书名称</view>
			<input class="itemview_text" type="text" value="" v-model="dataInf.name" />
		</view>
		<view class="itemview">
			<view class="itemview_title">证书编号</view>
			<input class="itemview_text" type="text" value="" v-model="dataInf.certNo" />
		</view>
		<view class="itemview">
			<view class="itemview_title">颁发机构</view>
			<input class="itemview_text" type="text" value="" v-model="dataInf.organization" />
		</view>
		<!-- <view class="itemview">
			<view class="itemview_title">颁发日期</view>
			<input class="itemview_text" type="text" value="" v-model="dataInf.awardTime" />
		</view> -->
		<view class="selectView_item">
			<view class="titleLabel">颁发日期</view>
			<view class="input_text" @tap="handleTap('picker89')">
				<view class="input_text_content">
					{{dataInf.awardTime}}
				</view>
				<view class="input_text_imgView">
					<image class="input_text_imgView_img" src="../../static/login/star3.png" mode=""></image>
				</view>
			</view>
			<bory-dateTimePicker ref='picker89' :indicatorStyle='indicatorStyle' :type='type' @change='confirm1'></bory-dateTimePicker>
		</view>


		<view class="itemview">
			<view class="itemview_title">证书照片</view>
			<image class="itemview_picture" :src="picStr.length>0?picStr:dataInf.certUrl?baseUrl+dataInf.certUrl:'../../static/mine/addpicture.png' "
			 mode="widthFix" @click="pictureClick()"></image>
		</view>

		
			<view class="commit_btn" @click="upData()">
				上传/保存
			</view>
		
	</view>
</template>

<script>
	var _this;
	export default {
		data() {
			return {
				id: '',
				picStr: '',
				type: 'date',
				baseUrl: "http://39.105.48.243:8080/crlink/",
				dataInf: {
					certNo: '',
					certUrl: '',
					organization: '', //证书颁发机构
					name: '',
					id: '',
					awardTime: '',
				}
			};
		},

		onLoad(e) {

			_this = this;
			if (e.id) {
				this.id = e.id
				this.getData()
			}

			this.baseUrl = getApp().globalData.baseUrl


		},
		computed: {

			indicatorStyle() {
				return {
					background: 'rgba(15, 128, 255, 0.4)',
					height: '40px',
				};
			},
		},
		methods: {
			getData() {

				var loginkey = uni.getStorageSync('loginKey');
				this.$api.post('resume!ajaxGetCertificateInfo.action', {
					loginKey: loginkey,
					certificationId: this.id,
				}).then(res => {
					if (res.res.status == 0) {
						this.dataInf = res.inf
					} else {
						uni.showModal({
							title: res.res.error
						})
					}
				})

			},

			upData() {

				if (this.id.length > 0) {
					//更新

					var dict = {
						loginKey: loginkey,
						certName: this.dataInf.name,
						organization: this.dataInf.organization,
						awardDate: this.dataInf.awardTime,
						certNo: this.dataInf.certNo
					}
					console.log("111111+_this.picStr=" + dict.certName);
					var loginkey = uni.getStorageSync('loginKey');
					if (this.picStr) {
						if (this.picStr.length > 0) {
							uni.uploadFile({
								url: this.baseUrl + 'app/resume!ajaxUpdateCertificateInfo.action', //仅为示例，非真实的接口地址
								filePath: this.picStr ? this.picStr : this.dataInf.certUrl ? this.baseUrl + this.dataInf.certUrl : '',
								name: 'certFile',
								formData: {
									'loginKey': loginkey,
									'certificationInfo': JSON.stringify({
										id: this.id,
										name: this.dataInf.name,
										organization: this.dataInf.organization,
										awardTime: this.dataInf.awardTime,
										certNo: this.dataInf.certNo
									})
								},
								success: (uploadFileRes) => {
									uni.showToast({
										title: "更改成功",
										success() {
											uni.navigateBack({})
										}
									})
								}

							})
						}
					} else {
						var loginkey = uni.getStorageSync('loginKey');
						this.$api.post('resume!ajaxUpdateCertificateInfo.action', {
							loginKey: loginkey,
							certificationInfo: JSON.stringify({
								id: this.id,
								name: this.dataInf.name,
								organization: this.dataInf.organization,
								awardTime: this.dataInf.awardTime,
								certNo: this.dataInf.certNo
							})

						}).then(res => {
							if (res.res.status == 0) {
								uni.showToast({
									title: "更改成功",
									success() {
										uni.navigateBack({

										})
									}
								})
							} else {
								uni.showModal({
									title: res.res.error
								})
							}
						})
					}
				} else {

					//添加
					var loginkey = uni.getStorageSync('loginKey');
					if (this.picStr) {
						if (this.picStr.length > 0) {

							var dict = {
								loginKey: loginkey,
								certName: this.dataInf.name,
								organization: this.dataInf.organization,
								awardDate: this.dataInf.awardTime,
								certNo: this.dataInf.certNo
							}

							console.log("111111+_this.picStr=" + dict.certName);

							const uploadTask = uni.uploadFile({
								url: this.baseUrl + 'app/resume!ajaxAddCertification.action', //仅为示例，非真实的接口地址
								filePath: _this.picStr ? _this.picStr : _this.dataInf.certUrl ? _this.baseUrl + _this.dataInf.certUrl : '../../static/image/mine/default_head_img@2x.png',
								name: 'certFile',
								formData: {
									'loginKey': loginkey,
									'certName': this.dataInf.name,
									'organization': this.dataInf.organization,
									'awardDate': this.dataInf.awardTime,
									'certNo': this.dataInf.certNo
								},
								success: (uploadFileRes) => {
									uni.showToast({
										title: "上传成功",
										success() {
											uni.navigateBack({})
										}
									})
								}

							})


							uploadTask.onProgressUpdate(function(res) {
								console.log('res=' + res);
								console.log('上传进度' + res.progress);
								console.log('已经上传的数据长度' + res.totalBytesSent);
								console.log('预期需要上传的数据总长度' + res.totalBytesExpectedToSend);
							});

						} else {
							uni.showToast({
								title: '没有添加图片'
							});
						}
					} else {

					}
				}


			},
			handleTap(picker) {
				this.$refs[picker].show()
			},

			confirm1(e) {
				this.dataInf.awardTime = e
			},

			pictureClick() {
				console.log("用户点击了从图库上传");
				uni.chooseImage({
					count: 1,
					sizeType: ["compressed"],
					success(response) {
						// 选择图片后, 返回的数据
						var fileUrl = response.tempFilePaths[0]
						console.log("用户点击了从图库上传222 =" + fileUrl);
						_this.picStr = fileUrl
					}
				})
			}
		}
	}
</script>

<style lang="scss">
	.itemview {
		padding: 10px;

		.itemview_title {
			height: 20px;
			line-height: 20px;
			font-size: 14px;
			color: #333333;
		}

		.itemview_text {
			margin-top: 10px;
			height: 20px;
			line-height: 20px;
			border-radius: 5px;
			background-color: #f5f7f8;
			font-size: 14px;
			color: #666666;
			padding: 5px;
		}

		.itemview_picture {
			margin: 10px 0px;
			// background-size:contain|cover;
			width: 100%;
			height: auto;
			border-radius: 5px;
			background-color: #f5f7f8;
		}
	}

	.bottomView{
		display: flex;
		align-items: center;
	}
	.commit_btn {
		margin-left: 10%;
		margin-top: 20px;
		margin-bottom: 20px;
		width: 80%;
		height: 40px;
		border-radius: 5px;
		background-color: #e8654b;
		text-align: center;
		line-height: 40px;
		color: #FFFFFF;
		font-size: 14px;
		flex: 1;
	}
	
	.delete_btn{
		margin-top: 20px;
		margin-bottom: 20px;
		width: 30%;
		margin-left: 10px;
		height: 40px;
		border-radius: 5px;
		background-color: #e8654b;
		text-align: center;
		line-height: 40px;
		color: #FFFFFF;
		font-size: 14px;
	}

	.selectView_item {
		flex: 1;

		.titleLabel {
			font-size: 14px;
			line-height: 15px;
			padding: 10px;
		}

		.input_text {
			margin: 0px 10px;
			height: 20px;
			font-size: 14px;
			background-color: #f5f7f8;
			border-radius: 5px;
			display: flex;
			color: #666666;
			align-items: center;
			padding: 5px 0px 5px 5px;

			.input_text_content {
				flex: 1;
				// height: 20px;
			}

			.input_text_imgView {
				width: 30px;
				height: 30px;
				background-color: #e8654b;
				border-top-right-radius: 5px;
				border-bottom-right-radius: 5px; //右下角
				display: flex;
				align-items: center;
				justify-content: center;

				.input_text_imgView_img {
					width: 8px;
					height: 5px;
				}
			}
		}
	}
</style>
