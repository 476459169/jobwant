<template>
	<view>

		<view class="item_views">
			<view class="item_title">
				学校名称
			</view>
			<view class="item">
				<input class="item_content" type="text" value="" v-model="dataInf.schoolName" />
			</view>
		</view>




		<view class="item_views">
			<view class="item_title">
				学{{zwf+zwf}}历
			</view>
			<view class="item" @tap="handleTap('picker333')">
				<view class="item_content">{{dataInf.selEducationContent}}</view>
				<view class="item_imgView">
					<image class="item_img" src="../../static/login/star3.png" mode=""></image>
				</view>
			</view>
		</view>
		<!-- <lb-picker ref="picker333" v-model="xltype" mode="selector" :list="educationArr">
		</lb-picker> -->
		<lb-picker ref="picker333" :props="myProps" mode="multiSelector" level='1' :list="educationArr" @confirm="handleConfirm">
		</lb-picker>

		<view class="item_views">
			<view class="item_title">
				学位证书
			</view>


			<view class="selectxl" @click="selectxlClick()">
				<view class="selectxl_item" :class="[haveXl==true?'isSelect':'']">
					有
				</view>
				<view class="selectxl_item" :class="[haveXl==true?'':'isSelect']">
					无
				</view>
			</view>

		</view>

		<view class="item_views">
			<view class="item_title">
				所学专业
			</view>
			<view class="item">
				<input class="item_content" type="text" value="" v-model="dataInf.major" />
			</view>
		</view>

		<view class="item_views">
			<view class="item_title">
				在校时间
			</view>
			<view class="item" @tap="handleTap('pickerdate3')">
				<view class="item_content">{{dataInf.enterDate}}</view>
				<view class="item_imgView">
					<image class="item_img" src="../../static/login/star3.png" mode=""></image>
				</view>
			</view>
			<view class="item_title" style="margin-left: 5px;">
				-
			</view>
			<view class="item" @tap="handleTap('pickerdate4')">
				<view class="item_content">{{dataInf.graduationDate}}</view>
				<view class="item_imgView">
					<image class="item_img" src="../../static/login/star3.png" mode=""></image>
				</view>
			</view>
		</view>
		<!-- <e-picker-plus ref="pickerdate1" @confirm="confirm1" mode="YM" :endRule=currentDate> </e-picker-plus>
		<e-picker-plus ref="pickerdate2" @confirm="confirm2" mode="YM" :endRule=currentDate> </e-picker-plus> -->
		<bory-dateTimePicker ref='pickerdate3' :indicatorStyle='indicatorStyle' :type='type' :datestring='workTime' @change='confirm1'></bory-dateTimePicker>
		<bory-dateTimePicker ref='pickerdate4' :indicatorStyle='indicatorStyle' :type='type' :datestring='workTime' @change='confirm2'></bory-dateTimePicker>
		<view class="bottom_view">
			<view class="bottom_view_qd" @click="save()">保存</view>
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
				myProps: {
					label: 'content',
					value: 'id',
					children: 'child'
				},
				zwf: '\u3000',
				currentDate: currentD,
				educationExpId: '',
				educationArr: [],
				haveXl: false,
				type: 'year-month',
				dataInf: {
					resumeId: '', //简历id
					schoolName: '',
					education: '', //学历id
					isHasDegree: 0, //是否有学位，1表示有，0表示没有
					major: '', //所学专业
					enterDate: '', //入学日期
					graduationDate: '', //毕业日期
					selEducationId: '', //选择学历id
					selEducationContent: '' //选中学历内容

				},

			};
		},


		onLoad(e) {
			this.dataInf.resumeId = e.id
			if (e.educationExpId) {
				this.educationExpId = e.educationExpId
				this.getMes()
			}
		},
		onShow() {
			this.getdownList()
			if (this.educationExpId) {
				this.getMes()
			}
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

			getdownList() {
				var loginkey = uni.getStorageSync('loginKey');
				this.$api.post('resume!ajaxGetEducationArr.action', {
					loginKey: loginkey
				}).then(res => {
					if (res.res.status == 0) {
						this.educationArr = res.inf.educationArr;
					} else {
						uni.showToast({
							title: res.res.error
						})
					}
				})
			},
			getMes(workid) {
				var loginkey = uni.getStorageSync('loginKey');
				this.$api.post('resume!ajaxGetEducationInfo.action', {
					loginKey: loginkey,
					educationExpId: this.educationExpId
				}).then(res => {
					if (res.res.status == 0) {
						this.dataInf = res.inf
					this.haveXl = res.inf.isHasDegree === 1?true:false
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

			handleConfirm(e) {

				this.dataInf.selEducationContent = e.value.map(item => item).join('-');
				for (var i = 0; i < e.item.length; i++) {
					if (i == 0) {
						console.log('industryInfoId=' + e.item[i].id);
						this.dataInf.selEducationId = e.item[i].id
						this.dataInf.education = e.item[i].id
					} else {


					}
				}
			},

			confirm1(e) {
				this.dataInf.enterDate = e
			},
			confirm2(e) {
				if (e === this.currentDate) {
					this.dataInf.graduationDate = '至今'
				} else {
					this.dataInf.graduationDate = e
				}
			},

			handleTap(picker) {
				this.$refs[picker].show()
			},
			selectxlClick() {
				this.haveXl = !this.haveXl
				if(this.haveXl === true){
					this.dataInf.isHasDegree = 1
				}else{
					this.dataInf.isHasDegree = 0
				}
			},

			save() {
				
				if(this.educationExpId){
					
						//更新
						var loginkey = uni.getStorageSync('loginKey');
							let dict = this.dataInf
							let workExpInf = JSON.stringify(dict)
							//添加
							this.$api.post('resume!ajaxUpdateEducationExpInfo.action', {
								loginKey: loginkey,
								educationExpInfo: workExpInf,
								educationExpId:this.educationExpId
							}).then(res => {
								if (res.res.status == 0) {
									uni.showToast({
										title: '保存成功',
										success() {
											uni.navigateBack({
							
											})
										}
									})
								} else {
									uni.showToast({
										title: res.res.error
									})
								}
							})
						
				}else{
					//添加
					var loginkey = uni.getStorageSync('loginKey');
					let dict = this.dataInf
					let workExpInf = JSON.stringify(dict)
					//添加
					this.$api.post('resume!ajaxAddEducation.action', {
						loginKey: loginkey,
						educationExpInfo: workExpInf
					}).then(res => {
						if (res.res.status == 0) {
							uni.showToast({
								title: '保存成功',
								success() {
									uni.navigateBack({
					
									})
								}
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
	}
</script>

<style lang="scss">
	.item_views {
		margin-top: 10px;
		display: flex;
		align-items: center;
		padding: 10px;

		.item_title {
			margin-right: 10px;
			font-size: 14px;
			height: 20px;
			line-height: 20px;
		}

		.item {
			display: flex;
			margin: 0px 0px 0px 0px;
			border-radius: 5px;
			background-color: #f5f7f8;
			font-size: 14px;
			color: #666666;

			flex: 1;

			.item_content {
				flex: 1;
				padding: 5px;

			}

			.item_imgView {
				width: 30px;
				height: 30px;
				background-color: #e8654b;
				border-top-right-radius: 5px;
				border-bottom-right-radius: 5px; //右下角

				display: flex;
				align-items: center;
				justify-content: center;

				.item_img {
					width: 8px;
					height: 5px;
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

	.desView {
		margin: 5px 10px;
		padding: 10px;
		width: calc(100vw - 40px);
		background-color: #f5f7f8;
		height: 60px;
		font-size: 14px;
	}

	.sw {
		transform: scale(0.7, 0.7);
	}

	.selectxl {
		display: flex;
		width: 100px;
		height: 20px;
		border-radius: 5px;
		border: 1px solid #e8654b;
		background-color: #e8654b;

		.selectxl_item {
			margin: 0px;
			font-size: 12px;
			color: #FFFFFF;
			border-radius: 5px;
			text-align: center;
			line-height: 20px;
			flex: 1;
			background-color: #FFFFFF;
		}

		.isSelect {
			background-color: #e8654b;
		}
	}
</style>
