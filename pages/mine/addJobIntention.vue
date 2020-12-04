<template>
	<view>

		<view class="item_views mar_top">
			<view class="item_title">
				期望职位
			</view>

			<view class="item">
				<input class="item_content" type="text" value="" v-model="dataInf.expectedPosition" />
			</view>
		</view>

		<lb-picker ref="picker333" v-model="type" mode="selector" :list="typelist">
		</lb-picker>


		<view class="item_views">
			<view class="item_title">
				求职状态
			</view>
			<view class="item" @tap="handleTap('picker33556')">
				<view class="item_content">{{dataInf.selJobStatusContent}}</view>
				<view class="item_imgView">
					<image class="item_img" src="../../static/login/star3.png" mode=""></image>
				</view>
			</view>
		</view>
		<lb-picker :props="myProps" ref="picker33556" mode="multiSelector" :list="jobStatusArr" @confirm="jobStatushandleConfirm">
		</lb-picker>

		<view class="item_views">
			<view class="item_title">
				工作城市
			</view>
			<view class="item" @tap="handleTap('picker334')">
				<view class="item_content">{{dataInf.workLocation}}</view>
				<view class="item_imgView">
					<image class="item_img" src="../../static/login/star3.png" mode=""></image>
				</view>
			</view>
		</view>

		<lb-picker ref="picker334"   mode="multiSelector" :list="cityArr" :level="2" @confirm="cityhandleConfirm">
		</lb-picker>

		<view class="item_views">
			<view class="item_title">
				薪资范围
			</view>
			<view class="item" @tap="handleTap('picker335')">
				<view class="item_content">{{dataInf.selSalaryContent}}</view>
				<view class="item_imgView">
					<image class="item_img" src="../../static/login/star3.png" mode=""></image>
				</view>
			</view>
		</view>
		<lb-picker :props="myProps" ref="picker335" mode="multiSelector" :list="salaryArr" @confirm="salaryhandleConfirm">
		</lb-picker>

		<view class="item_views">
			<view class="item_title">
				工作性质
			</view>
			<view class="item" @tap="handleTap('picker336')">
				<view class="item_content">{{dataInf.selWorkNatureContent}}</view>
				<view class="item_imgView">
					<image class="item_img" src="../../static/login/star3.png" mode=""></image>
				</view>
			</view>
		</view>
		<lb-picker :props="myProps" ref="picker336" mode="multiSelector" :list="worknatureArr" @confirm="workhandleConfirm">
		</lb-picker>


		<view class="moreSelect">
			<view class="moreSelect_title">
				期待公司
			</view>
			<view class="moreSelect_title_views">
				<view class="moreSelect_title_views_item" v-for="(item,index) in companynatureArr" :key="index" @click="selectClick(item)">
					<view class="moreSelect_title_views_item_btnNormal" :class="[selectArr.indexOf(item)!==-1?'moreSelect_title_views_item_btnSelected':'']">

					</view>
					<view class="moreSelect_title_views_item_btnTitleNormal" :class="[selectArr.indexOf(item)!==-1?'moreSelect_title_views_item_btnTitleSelected':'']">
						{{item.content}}
					</view>
				</view>
			</view>
		</view>



		<view class="bottom_view">
			<view class="bottom_view_cz" @click="dele()">删除</view>
			<view class="bottom_view_qd" @click="save()">保存</view>
		</view>
	</view>
</template>

<script>
	import areaData from '../home/area-data-min.js'
	export default {
		data() {
			return {
				myProps: {
					label: 'content',
					value: 'id',
					children: 'child'
				},
				id: '',
				selectArr:[],
				wantedIntentionId: '',
				cityArr: areaData, //工作城市
				salaryArr: [], //薪资范围
				worknatureArr: [], //工作性质
				companynatureArr: [] ,//期待公司
				jobStatusArr:[],//求职状态
				dataInf: {
					resumeId: '',
					workLocation: '', //工作地点
					selWorkNatureContent: '', //选中的工作性质内容
					selWorkNatureId: '', //工作性质id
					selSalaryContent: '', //薪水
					selSalaryId: '', //薪水id
					expectedPosition: '', //期望职位
					selJobStatusId:'',
					selJobStatusContent:'',//求职状态
				}

			};
		},

		onLoad(e) {
			this.id = e.id
			console.log('id = '+e.id);
			this.getdownList()
			if (e.wantedIntentionId) {
				this.wantedIntentionId = e.wantedIntentionId
				this.getdata()
			}

		},
		methods: {
			getdownList() {
				var loginkey = uni.getStorageSync('loginKey');
				this.$api.post('resume!ajaxGetWantedIntentionDropdownInfo.action', {
					loginKey: loginkey
				}).then(res => {
					if (res.res.status == 0) {
						this.salaryArr = res.inf.salaryArr
						this.worknatureArr = res.inf.worknatureArr
						this.companynatureArr = res.inf.companynatureArr
						this.jobStatusArr = res.inf.jobStatusArr
						
					} else {
						uni.showToast({
							title: res.res.error
						})
					}
				})
			},

			getdata() {
				var loginkey = uni.getStorageSync('loginKey');
				this.$api.post('resume!ajaxGetWantedIntentionInfo.action', {
					loginKey: loginkey,
					wantedIntentionId: this.wantedIntentionId
				}).then(res => {
					if (res.res.status == 0) {
						this.dataInf = res.inf
						this.companynatureArr = res.inf.companyNatureArr
						for (var i = 0; i < this.companynatureArr.length; i++) {
							let item1 =this.companynatureArr[i]
							if(item1.isSelected === 1){
								this.selectArr.push(item1)
							}
						}
					} else {
						uni.showToast({
							title: res.res.errMsg
						})
					}
				})
			},
			
			
			jobStatushandleConfirm(e){
				this.dataInf.selJobStatusContent = e.value.map(item => item).join('-');
				for (var i = 0; i < e.item.length; i++) {
					if (i == 0) {
						console.log('jobStatusid=' + e.item[i].id);
						this.dataInf.selJobStatusId = e.item[i].id
					} 
				}
			},

			cityhandleConfirm(e){
				this.dataInf.workLocation = e.value[1];
			},
			salaryhandleConfirm(e) {
				this.dataInf.selSalaryContent = e.value.map(item => item).join('-');
				for (var i = 0; i < e.item.length; i++) {
					if (i == 0) {
						console.log('selSalaryId=' + e.item[i].id);
						this.dataInf.selSalaryId = e.item[i].id
					} 
				}

			},
			workhandleConfirm(e) {
				this.dataInf.selWorkNatureContent = e.value.map(item => item).join('-');
				for (var i = 0; i < e.item.length; i++) {
					if (i == 0) {
						console.log('classificationContent=' + e.item[i].id);
						this.dataInf.selWorkNatureId = e.item[i].id
					} 
				}
			},

			handleTap(picker) {
				this.$refs[picker].show()
			},

			selectClick(item) {
				if (this.selectArr.indexOf(item) == -1) {
						
					if(item.content === '全部'){
						this.selectArr = []
					}else{
						for (var i = 0; i < this.selectArr.length; i++) {
							let item1 = this.selectArr[i]
							if(item1.content === '全部'){
								var index1 = this.selectArr.indexOf(item1);
								this.selectArr.splice(index1, 1)
							}
						}
					}
					
					this.selectArr.push(item)
				} else {
					var index1 = this.selectArr.indexOf(item);
					this.selectArr.splice(index1, 1)
				}
			},
			
				
			save(){
				let selectIdArr = []
				for (var i = 0; i < this.selectArr.length; i++) {
					let item1 = this.selectArr[i]
					selectIdArr.push(item1.id)
				}
				let dict = {
					resumeId:this.id,
					expectedPosition:this.dataInf.expectedPosition,
					expectedSalary:this.dataInf.selSalaryId,
					workLocation:this.dataInf.workLocation,
					workNature:this.dataInf.selWorkNatureId,
					companyNature:selectIdArr,
					jobStatus:this.dataInf.selJobStatusId
				}
					
				if(this.wantedIntentionId){
					//更新
					var loginkey = uni.getStorageSync('loginKey');
					this.$api.post('resume!ajaxUpdateWantedIntention.action', {
						loginKey: loginkey,
						wantedIntentionId:this.wantedIntentionId,
						jobWantedIntentionInfo: JSON.stringify(dict)
					}).then(res => {
						if (res.res.status == 0) {
							uni.showToast({
								title:'更新成功',
								complete() {
									uni.navigateBack({})
								}
							})
						} else {
							uni.showToast({
								title: res.res.error
							})
						}
					})
				}else{
					var loginkey = uni.getStorageSync('loginKey');
					this.$api.post('resume!ajaxAddJobWantedIntention.action', {
						loginKey: loginkey,
						resumeId:this.id,
						jobWantedIntentionInfo:JSON.stringify(dict)
					}).then(res => {
						if (res.res.status == 0) {
							uni.showToast({
								title:'添加成功',
								complete() {
									uni.navigateBack({})
								}
							})
						} else {
							uni.showToast({
								title: res.res.error
							})
						}
					})
				}
			},
			
			dele(){
				var loginkey = uni.getStorageSync('loginKey');
				this.$api.post('resume!ajaxDeleteWantedIntention.action', {
					loginKey: loginkey,
					wantedIntentionId: this.wantedIntentionId
				}).then(res => {
					if (res.res.status == 0) {
						uni.showToast({
							title:'删除成功',
							complete() {
								uni.navigateBack({})
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
			margin: 10px 10px 10px 0px;
			background-color: #e8654b;
			color: #FFFFFF;
			font-size: 17px;
			text-align: center;
			line-height: 30px;

		}
	}

	.mar_top {
		margin-top: 20px;
	}


	.moreSelect {
		display: flex;
		padding: 10px;

		.moreSelect_title {
			font-size: 14px;
			height: 20px;
			line-height: 20px;
			margin-top: 10px;
			// width: 100%;
		}

		.moreSelect_title_views {
			display: flex;
			flex-wrap: wrap;
			flex: 1;

			.moreSelect_title_views_item {
				margin-left: 10px;
				margin-top: 10px;
				height: 10px;
				display: flex;
				align-items: center;

				.moreSelect_title_views_item_btnNormal {
					width: 7px;
					height: 7px;
					border: 1px solid #000000;
					background-color: #FFFFFF;
				}

				.moreSelect_title_views_item_btnSelected {
					width: 7px;
					height: 7px;
					border: 1px solid #e8654b;
					background-color: #e8654b;
				}

				.moreSelect_title_views_item_btnTitleNormal {
					margin-left: 3px;
					font-size: 12px;
				}

				.moreSelect_title_views_item_btnTitleSelected {
					margin-left: 3px;
					color: #e8654b;
				}

			}
		}
	}
</style>
