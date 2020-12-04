<template>
	<view>

		<view class="item_views">
			<view class="item_title">
				项目名称
			</view>
			<view class="item">
				<input class="item_content" type="text" value="" v-model="dataInf.name" />
			</view>
		</view>



		<view class="item_views">
			<view class="item_title">
				项目分类
			</view>
			<view class="item" @tap="handleTap('picker33333')">
				<view class="item_content">{{dataInf.typeContent}}</view>
				<view class="item_imgView">
					<image class="item_img" src="../../static/login/star3.png" mode=""></image>
				</view>
			</view>
		</view>
		<!-- <lb-picker ref="picker33333" v-model="type" mode="selector" :list="typelist">
			
		</lb-picker> -->
		<lb-picker ref="picker33333" :props="myProps" mode="multiSelector" level='1' :list="projectTypeArr" @confirm="handleConfirm">
		</lb-picker>

		<view class="item_views">
			<view class="item_title">
				项目类型
			</view>
			<view class="item" @tap="handleTap('picker333331')">
				<view class="item_content">{{dataInf.classificationContent}}</view>
				<view class="item_imgView">
					<image class="item_img" src="../../static/login/star3.png" mode=""></image>
				</view>
			</view>
		</view>
		<!-- 	<lb-picker ref="picker33333" v-model="type" mode="selector" :list="typelist">
		</lb-picker> -->
		<lb-picker ref="picker333331" :props="myProps" mode="multiSelector" level='1' :list="classificationArr" @confirm="handleConfirm1">
		</lb-picker>


		<view class="item_views">
			<view class="item_title">
				项目时间
			</view>
			<view class="item" @tap="handleTap('pickerdate5')">
				<view class="item_content">{{dataInf.startDate}}</view>
				<view class="item_imgView">
					<image class="item_img" src="../../static/login/star3.png" mode=""></image>
				</view>
			</view>
			<view class="item_title" style="margin-left: 5px;">
				-
			</view>
			<view class="item" @tap="handleTap('pickerdate6')">
				<view class="item_content">{{dataInf.finishDate}}</view>
				<view class="item_imgView">
					<image class="item_img" src="../../static/login/star3.png" mode=""></image>
				</view>
			</view>
		</view>
		<!-- <e-picker-plus ref="pickerdate1" @confirm="confirm1" mode="YM" :endRule=currentDate> </e-picker-plus>
		<e-picker-plus ref="pickerdate2" @confirm="confirm2" mode="YM" :endRule=currentDate> </e-picker-plus> -->
		<bory-dateTimePicker ref='pickerdate5' :indicatorStyle='indicatorStyle' :type='type' :datestring='workTime' @change='confirm1'></bory-dateTimePicker>
		<bory-dateTimePicker ref='pickerdate6' :indicatorStyle='indicatorStyle' :type='type' :datestring='workTime' @change='confirm2'></bory-dateTimePicker>

		<view class="item_views">
			<view class="item_title">
				所属公司
			</view>
			<view class="item">
				<input class="item_content" type="text" value="" v-model="dataInf.company" />
			</view>
		</view>


		<view class="item_views">
			<view class="item_title">
				项目描述
			</view>
		</view>
		<textarea class="desView" maxlength="-1" type="text" value="" v-model="dataInf.projectDescription" />

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
				currentDate: currentD,
				switchValue: false,
				
				projectExpId:'',//项目经历ID
				projectTypeArr:[],
				classificationArr:[],
				type: 'year-month',
				dataInf:{
					resumeId: '', //简历id
					typeContent:'',//项目分类内容
					selTypeId:'',//选中的项目分类id
					classificationContent:'',//选中的项目类别内容
					selClassificationId:'',//选中的项目类别id
					name:'',//项目内容
					projectDescription:'',//项目描述
					finishDate:'',//项目结束时间
					startDate:'',//项目开始时间
					company:'',//所属公司名称
				},
				
				/*
				*/
			};
		},


		onLoad(e) {
			if(e.projectExpId){
				this.projectExpId = e.projectExpId
			}
				
			if(e.id){
				this.dataInf.resumeId = e.id
				this.getMes()
			}
			
			this.getdownList()
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
				this.$api.post('resume!ajaxGetProjectExpDropdownInfo.action', {
					loginKey: loginkey
				}).then(res => {
					if (res.res.status == 0) {
						this.classificationArr = res.inf.classificationArr;
						this.projectTypeArr = res.inf.projectTypeArr;
					} else {
						uni.showToast({
							title: res.res.error
						})
					}
				})
			},
			getMes(workid) {
				var loginkey = uni.getStorageSync('loginKey');
				this.$api.post('resume!ajaxGetProjectExperienceInfo.action', {
					loginKey: loginkey,
					projectExpId: this.projectExpId
				}).then(res => {
					if (res.res.status == 0) {
						this.dataInf = res.inf
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
			switchClick() {
				this.switchValue = !this.switchValue;
				console.log('this.switch = ' + this.switchValue);
			},
			
			handleConfirm(e) {
			
				this.dataInf.typeContent = e.value.map(item => item).join('-');
				for (var i = 0; i < e.item.length; i++) {
					if (i == 0) {
						console.log('industryInfoId=' + e.item[i].id);
						this.dataInf.selTypeId = e.item[i].id
					} else {
			
			
					}
				}
			},
			handleConfirm1(e) {
			
				this.dataInf.classificationContent = e.value.map(item => item).join('-');
				for (var i = 0; i < e.item.length; i++) {
					if (i == 0) {
						console.log('classificationContent=' + e.item[i].id);
						this.dataInf.selClassificationId = e.item[i].id
					} else {
			
			
					}
				}
			},
			
			confirm1(e) {
				this.dataInf.startDate = e
				// console.log(e.result)
			},
			confirm2(e) {
				if(e === this.currentDate){
					this.dataInf.finishDate='至今'
				}else{
					this.dataInf.finishDate = e
				}
			},
			save(){
					
				if(this.projectExpId){
					//更新
					var loginkey = uni.getStorageSync('loginKey');
						let dict = {
							resumeId:this.dataInf.resumeId,
							projectName:this.dataInf.name,
							classification:this.dataInf.selClassificationId,
							type:this.dataInf.selTypeId,
							startDate:this.dataInf.startDate,
							finishDate:this.dataInf.finishDate,
							company:this.dataInf.company,
							projectDescription:this.dataInf.projectDescription
						}
						let workExpInf = JSON.stringify(dict)
						this.$api.post('resume!ajaxUpdateProjectExperience.action', {
							loginKey: loginkey,
							projectExpInfo: workExpInf,
							projectExpId:this.projectExpId
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
						let dict = {
							resumeId:this.dataInf.resumeId,
							projectName:this.dataInf.name,
							classification:this.dataInf.selClassificationId,
							type:this.dataInf.selTypeId,
							startDate:this.dataInf.startDate,
							finishDate:this.dataInf.finishDate,
							company:this.dataInf.company,
							projectDescription:this.dataInf.projectDescription
						}
						let workExpInf = JSON.stringify(dict)
						//添加
						this.$api.post('resume!ajaxAddProjectExp.action', {
							loginKey: loginkey,
							projectExpInfo: workExpInf,
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
		z-index: 999;

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
		padding: 10px 10px 80px 10px;
		width: calc(100vw - 40px);
		background-color: #f5f7f8;
		height: 600px;
		font-size: 14px;
	}

	.sw {
		transform: scale(0.7, 0.7);
	}

	.timefont {
		font-size: 12px;
	}
</style>
