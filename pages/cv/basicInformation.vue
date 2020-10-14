<template>
	<view>
		<view class="selectView">
			<view class="selectView_item">
				<view class="titleLabel">姓名</view>
				<view class="input_text">
					<input class="input_text_content" type="text" value="" v-model="realName" />
				</view>
			</view>

			<!-- 	<view class="selectView_item">
				<view class="titleLabel">性别</view>
				<view class="input_text">
					<input class="input_text_content" type="text" value="" v-model="sexvalue" />
				</view>
			</view> -->


			<view class="selectView_item">
				<view class="titleLabel">性别</view>
				<view class="input_text" @tap="handleTap('picker111122')">
					<view class="input_text_content">
						{{sexvalue}}
					</view>
					<view class="input_text_imgView">
						<image class="input_text_imgView_img" src="../../static/login/star3.png" mode=""></image>
					</view>
				</view>
				<lb-picker ref="picker111122" v-model="sex" mode="selector" :list="sexlist" @confirm="handleConfirm">
				</lb-picker>
			</view>

		</view>

		<view class="selectView">
			<view class="selectView_item">
				<view class="titleLabel">电话</view>
				<view class="input_text">
					<input class="input_text_content" type="text" value="" v-model="contactPhoneNum" />
				</view>
			</view>

			<view class="selectView_item">
				<view class="titleLabel">邮箱</view>
				<view class="input_text">
					<input class="input_text_content" type="text" value="" v-model="email" />
				</view>
			</view>
		</view>


		<view class="selectView">

			<view class="selectView_item">
				<view class="titleLabel">出生年月</view>
				<view class="input_text" @tap="handleTap('pickerdate11')">
					<view class="input_text_content">
						{{bornTime}}
					</view>
					<view class="input_text_imgView">
						<image class="input_text_imgView_img" src="../../static/login/star3.png" mode=""></image>
					</view>
				</view>
				<e-picker-plus ref="pickerdate11" @confirm="confirm1" mode="YM"  :endRule=currentDate> </e-picker-plus>
			</view>


		</view>

		<view class="selectView">
			<view class="selectView_item">
				<view class="titleLabel">参加工作时间</view>
				<view class="input_text" @tap="handleTap('pickerdate11223')">
					<view class="input_text_content">
						{{workTime}}
					</view>
					<view class="input_text_imgView">
						<image class="input_text_imgView_img" src="../../static/login/star3.png" mode=""></image>
					</view>
				</view>
				<e-picker-plus ref="pickerdate11223" @confirm="confirm2" mode="YM" :endRule=currentDate> </e-picker-plus>
			</view>
		</view>

		<view class="selectView">
			<view class="selectView_item">
				<view class="titleLabel">现居住城市</view>
				<view class="input_text">
					<input class="input_text_content" type="text" value="" v-model="livingLocation" />
				</view>
			</view>
		</view>

		<view class="selectView">
			<view class="selectView_item">
				<view class="titleLabel">户口所在地</view>
				<view class="input_text">
					<input class="input_text_content" type="text" value="" v-model="hukouLocation" />
				</view>
			</view>
		</view>

		<view class="bottom_view">
			<view class="bottom_view_qd" @click="saveClick()">保存</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			const currentD = this.getDate({
				format: true
			})
			return {
				currentDate: currentD,
				sexlist: [{
					label: '男',
					value: '0'
				}, {
					label: '女',
					value: '1'
				}],
				realName: '',
				livingLocation: '',
				bornTime: '',
				hukouLocation: '',
				contactPhoneNum: '',
				sex: 0,
				workTime: '',
				email: '',
				dataInfo: Object,
				id: null
			};
		},

		onShow() {
			this.getBasicInfo()
		},
		computed: {

			sexvalue() {
				if (this.sex == 0) {
					return '男'
				} else {
					return '女'
				}
			}
		},

		methods: {
			getBasicInfo() {
				var loginkey = uni.getStorageSync('loginKey');
				this.$api.post('resume!ajaxGetUserUpdateInfo.action', {
					loginKey: loginkey
				}).then(res => {
					if (res.res.status == 0) {
						this.dataInfo = res.inf
						this.realName = this.dataInfo.realName,
							this.livingLocation = this.dataInfo.livingLocation,
							this.bornTime = this.dataInfo.bornTime,
							this.hukouLocation = this.dataInfo.hukouLocation,
							this.contactPhoneNum = this.dataInfo.contactPhoneNum,
							this.sex = this.dataInfo.sex,
							this.workTime = this.dataInfo.workTime,
							this.email = this.dataInfo.email
					} else {
						uni.showToast({
							title: res.res.error
						})
					}
				})
			},

			getDate(type) {
				const date = new Date();

				let year = date.getFullYear();
				let month = date.getMonth() + 1;
				let day = date.getDate();

				if (type === 'start') {
					year = year - 60;
				} else if (type === 'end') {
					year = year + 2;
				}
				month = month > 9 ? month : '0' + month;;
				day = day > 9 ? day : '0' + day;
				let ztime = `${year}-${month}`
				return ztime;
			},

			handleTap(picker) {
				this.$refs[picker].show()
			},
			confirm1(e) {
				this.bornTime = e.result
			},
			confirm2(e) {
				this.workTime = e.result
			},
			handleConfirm(e) {
				this.sex = e.item.value;
			},
			saveClick() {
				var loginkey = uni.getStorageSync('loginKey');
				let dic = {
					realName: this.realName,
					livingLocation: this.livingLocation,
					bornTime: this.bornTime,
					hukouLocation: this.hukouLocation,
					contactPhoneNum: this.contactPhoneNum,
					sex: this.sex,
					workTime: this.workTime,
					email: this.email,
				}
				let updic = {
					loginKey: loginkey,
					userInfo: JSON.stringify(dic)
				};
				this.$api.post('resume!ajaxUpdateUserBaseInfo.action', updic).then(res => {
					if (res.res.status == 0) {
						uni.navigateBack({
							
						})
					} else {
						uni.showToast({
							title: res.res.error
						})
					}
				})
			}

		}



	}
</script>

<style lang="scss">
	.selectView {
		display: flex;
		align-items: center;
		margin-bottom: 10px;

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



	}

	.bottom_view {
		position: fixed;
		bottom: 0px;
		height: 50px;
		display: flex;
		justify-content: center;
		width: 100%;

		.bottom_view_cz {
			border: 2px solid #e8654b;
			background-color: #fbc9bc;
			color: #e8654b;
			font-size: 17px;
			border-radius: 5px;
			width: 80px;
			height: 30px;
			margin: 10px;
			text-align: center;
			line-height: 30px;
		}

		.bottom_view_qd {
			border: 2px solid #e8654b;
			border-radius: 5px;
			height: 30px;
			flex: 1;
			margin: 10px 10px 10px 10px;
			background-color: #e8654b;
			color: #FFFFFF;
			font-size: 17px;
			text-align: center;
			line-height: 30px;

		}
	}
</style>
