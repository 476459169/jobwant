<template>
	<view>


		<!-- <view class="itemview" style="margin-top: 10px;">
			<view class="itemview_title">简历名称<text class="btxet">{{zwf}}⦁</text></view>
			<input class="itemview_text" type="text" value="" />
		</view>
		 -->
		<view class="plateItemview">
			<view class="plateItemview_titleView">
				<view class="plateItemview_titleView_title">简历名称<text class="btxet">{{zwf}}⦁</text></view>
				<!-- <view class="plateItemview_titleView_mes">必填</view> -->
				<view class="plateItemview_titleView_noneview"></view>
				<image class="rightBtn" src="../../static/cv/bj.png" mode="" @click="editName()"></image>
			</view>

			<view class="" v-if="data.title.length>0">
				<view class="plateItemview_titleView_title" style="color: #666666;">
					{{data.title}}
				</view>
			</view>
		</view>


		<view class="selectView">

			<view class="selectView_item">
				<view class="titleLabel">简历状态</view>
				<view class="input_text" @tap="handleTap('picker11111')">
					<view class="input_text_content">
						{{status}}
					</view>
					<view class="input_text_imgView">
						<image class="input_text_imgView_img" src="../../static/login/star3.png" mode=""></image>
					</view>
				</view>
				<lb-picker ref="picker11111" mode="selector" :list="cvtype" @confirm="handleConfirm">
				</lb-picker>
			</view>

			<view class="selectView_item">
				<view class="titleLabel">设为默认</view>
				<view class="input_text" @tap="handleTap('picker111111')">
					<view class="input_text_content">
						{{zd}}
					</view>
					<view class="input_text_imgView">
						<image class="input_text_imgView_img" src="../../static/login/star3.png" mode=""></image>
					</view>
				</view>
				<lb-picker ref="picker111111" mode="selector" :list="zdArr" @confirm="handleConfirm1">
				</lb-picker>
			</view>


		</view>


		<view class="plateItemview">
			<view class="plateItemview_titleView" @click="wantjobClick()">
				<view class="plateItemview_titleView_title">求职意向<text class="btxet">{{zwf}}⦁</text></view>
				<!-- <view class="plateItemview_titleView_mes">必填</view> -->
				<view class="plateItemview_titleView_noneview"></view>
				<image class="rightBtn" src="../../static/cv/sz.png" mode=""></image>
			</view>

			<view class="" v-if="data.wantedIntention.expectedPosition.length>0">
				<view class="view_item">
					<image class="view_item_img" src="../../static/cv/sj.png" mode=""></image>
					<view class="view_item_text">{{data.wantedIntention.expectedPosition}}</view>
				</view>
				<view class="" >
					<view class="view_item">
						<image class="view_item_img" src="../../static/cv/b.png" mode=""></image>
						<view class="view_item_text">{{data.wantedIntention.jobStatusContent}}</view>
					</view>
				</view>
			</view>
		</view>

		<view class="plateItemview">
			<view class="plateItemview_titleView">
				<view class="plateItemview_titleView_title">
					工作/实习经验<text class="btxet">{{zwf}}⦁</text>
				</view>
				<!-- 	<view class="plateItemview_titleView_mes">
					必填
				</view> -->
				<view class="plateItemview_titleView_noneview"></view>
				<image class="rightBtn" src="../../static/cv/tj.png" mode="" @click="addworkExperienceClick()"></image>
			</view>

			<view v-if="data.workExpArr.length>0">
				<view class="work_experienceView" v-for="(item,index) in data.workExpArr" :key="index">
					<view class="" v-if="index<showWorkExperienceNumb">
						<view class="view_item">
							<image class="view_item_img" src="../../static/cv/gs.png"></image>
							<view class="view_item_text">{{item.companyName}}</view>
							<image class="rightBtn" src="../../static/cv/bj.png" mode="" @click="workExperienceClick(item,index)"></image>
						</view>
						<view class="work_experienceView_time">{{item.workTime}}</view>
						<view class="work_experienceView_jobview">
							<view class="work_experienceView_jobview_title">{{item.positionName}}</view>
							<view class="work_experienceView_jobview_salary">{{item.monthSalary}}/k</view>
						</view>
						<view class="work_experienceView_detail">{{item.workDescription}}</view>
					</view>
				</view>
			</view>

			<view v-if="data.workExperience.length>2" class="down_view" @click="dowmViewClick()">
				<image v-if="showWorkExperience==true" class="down_view_img" src="../../static/cv/upcrrow.png" mode=""></image>
				<image v-else class="down_view_img" src="../../static/cv/downcrrow.png" mode=""></image>

			</view>
		</view>

		<view class="plateItemview">
			<view class="plateItemview_titleView">
				<view class="plateItemview_titleView_title">教育经历<text class="btxet">{{zwf}}⦁</text></view>
				<!-- <view class="plateItemview_titleView_mes">必填</view> -->
				<view class="plateItemview_titleView_noneview"></view>
				<image class="rightBtn" src="../../static/cv/tj.png" @click="addeducationClick()"></image>
			</view>

			<view v-if="data.educationArr.length>0">
				<view class="work_experienceView" v-for="(item,index) in data.educationArr" :key="index">
					<view class="view_item">
						<image class="view_item_img" src="../../static/cv/b.png"></image>
						<view class="view_item_text">{{item.schoolName}}</view>
						<image class="rightBtn" src="../../static/cv/bj.png" @click="educationClick(item)"> </image>
					</view>
					<view class="work_experienceView_time">{{item.studyDate}}</view>
					<view class="work_experienceView_jobview">
						<view class="work_experienceView_jobview_title">
							{{item.education+'&nbsp|&nbsp'+item.major}}
						</view>
					</view>
				</view>
			</view>
		</view>

		<view class="plateItemview">
			<view class="plateItemview_titleView">
				<view class="plateItemview_titleView_title">项目经历</view>
				<view class="plateItemview_titleView_noneview"></view>
				<image class="rightBtn" src="../../static/cv/tj.png" @click="addprojecClick()"></image>
			</view>

			<view v-if="data.projectExpArr.length>0">
				<view class="work_experienceView" v-for="(item,index) in data.projectExpArr" :key="index">
					<view class="view_item">
						<image class="view_item_img" src="../../static/cv/b.png"></image>
						<view class="view_item_text">{{item.name}}</view>
						<image class="rightBtn" src="../../static/cv/bj.png" @click="projecClick(item)"></image>
					</view>
					<view class="work_experienceView_time"> {{item.projectDate}}</view>
					<!-- <view class="work_experienceView_jobview">
						<view class="work_experienceView_jobview_title" style="color: #666666;">
							{{item.description}}
						</view>
					</view> -->
					<view class="work_experienceView_detail">{{item.description}}</view>
				</view>
			</view>



		</view>

		<view class="plateItemview">
			<view class="plateItemview_titleView">
				<view class="plateItemview_titleView_title">获得证书</view>
				<view class="plateItemview_titleView_noneview"></view>
				<image class="rightBtn" src="../../static/cv/tj.png" @click="cerClick()"></image>
			</view>
			<view v-if="data.certificateArr.length>0">
				<view v-for="(item,index) in data.certificateArr" :key="index">
					<view class="view_item">
						<image class="view_item_img" src="../../static/cv/b.png"></image>
						<view class="view_item_text">{{item.name}}</view>
					</view>
				</view>
			</view>
		</view>

		<!-- 
		<view class="plateItemview">
			<view class="plateItemview_titleView">
				<view class="plateItemview_titleView_title">培训经历</view>
				<view class="plateItemview_titleView_noneview"></view>
				<image class="rightBtn" src="../../static/cv/tj.png"></image>
			</view>
			<view v-if="data.trainingExperience.length>0">
				<view class="work_experienceView" v-for="(item,index) in data.trainingExperience" :key="index">
					<view class="view_item">
						<image class="view_item_img" src="../../static/cv/gs.png"></image>
						<view class="view_item_text">{{item.trainingJG}}</view>
						<image class="rightBtn" src="../../static/cv/bj.png"></image>
					</view>
					<view class="work_experienceView_time"> {{item.beginTime+'-'+item.endTime}}</view>
				</view>
			</view>
		</view>

		<view class="plateItemview">
			<view class="plateItemview_titleView">
				<view class="plateItemview_titleView_title">语言能力</view>
				<view class="plateItemview_titleView_noneview"></view>
				<image class="rightBtn" src="../../static/cv/tj.png"></image>
			</view>
			<view v-if="data.language.length>0">
				<view v-for="(item,index) in data.language" :key="index">
					<view class="view_item">
						<image class="view_item_img" src="../../static/cv/b.png"></image>
						<view class="view_item_text">{{item.language}}</view>
					</view>
				</view>
			</view>
		</view> -->


		<view class="plateItemview">
			<view class="plateItemview_titleView">
				<view class="plateItemview_titleView_title">自我评价</view>
				<view class="plateItemview_titleView_noneview"></view>
				<image class="rightBtn" src="../../static/cv/bj.png" @click="assessmen()"></image>
			</view>
			<view class="self_pj">
				{{data.selfEvaluation}}
			</view>
		</view>


		<view class="reload_time">
			{{'简历更新时间：'+data.reloadTime}}
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				showWorkExperience: false,
				showWorkExperienceNumb: 2,
				status: '公开',
				cvtype: [{
					label: '保密',
					value: '0'
				}, {
					label: '公开',
					value: '1'
				}],
				zd: '否',
				zdArr: [{
					label: '是',
					value: '1'
				}, {
					label: '否',
					value: '0'
				}],
				zwf: '\u0020',
				data: Object,
			};
		},

		onLoad(e) {  
			this.id = e.id
			console.log('id=' + e.id);
		},

		onShow() {
			this.getDetailMes()
		},
		methods: {

			getDetailMes() {
				var loginkey = uni.getStorageSync('loginKey');
				this.$api.post('resume!ajaxGetOnlineResumeInfo.action', {
					loginKey: loginkey,
					resumeId: this.id
				}).then(res => {
					if (res.res.status == 0) {
						this.data = res.inf
						this.status = res.inf.isPublic===1?'公开':'保密'
						this.zd = res.inf.isDefault ===1?'是':'否'
					} else {
						uni.showToast({
							title: res.res.error
						})
					}

				})
			},

			handleTap(picker) {
				this.$refs[picker].show()
			},
			handleConfirm(e) {
				this.status = e.item.label;
				console.log('handconfirm' + e.item.value);
				var loginkey = uni.getStorageSync('loginKey');
				this.$api.post('resume!ajaxUpdateResumePublic.action', {
					loginKey: loginkey,
					resumeId: this.id,
					isPublic: e.item.value
				}).then(res => {
					if (res.res.status == 0) {

					} else {
						uni.showToast({
							title: res.res.error
						})
					}

				})

			},
			handleConfirm1(e) {
				this.zd = e.item.label;
				console.log('handconfirm' + e.item.value);
				var loginkey = uni.getStorageSync('loginKey');
				this.$api.post('resume!ajaxUpdateResumeDefault.action', {
					loginKey: loginkey,
					resumeId: this.id,
					isDefault: e.item.value
				}).then(res => {
					if (res.res.status == 0) {
					} else {
						uni.showToast({
							title: res.res.error
						})
					}

				})
			},
			dowmViewClick() {
				this.showWorkExperience = !this.showWorkExperience
				if (this.showWorkExperience == true) {
					this.showWorkExperienceNumb = this.data.workExperience.length
				} else {
					this.showWorkExperienceNumb = 2
				}
				console.log('this.showWorkExperience = ' + this.showWorkExperienceNumb);
			},

			editName() {
				uni.navigateTo({
					url: './vcName?id=' + this.id
				})
			},

			educationClick(item) {
				uni.navigateTo({
					url: './educationExperience?id=' + this.id + '&educationExpId=' + item.id
				})
			},


			addeducationClick() {
				uni.navigateTo({
					url: './educationExperience?id=' + this.id
				})
			},

			addworkExperienceClick() {

				uni.navigateTo({
					url: './workExperience?id=' + this.id
				})
			},

			cerClick() {
				uni.navigateTo({
					url: './CertificateAssociated?id=' + this.id
				})
			},

			workExperienceClick(item, index) {
				console.log('index = ' + index);

				console.log("workExperience=" + item.id);
				if (item) {
					uni.navigateTo({
						url: './workExperience?workExpId=' + item.id + '&id=' + this.id
					})
				}

			},

			wantjobClick() {
				// uni.navigateTo({
				// 	url: '../mine/jobManage?id=' + this.id
				// })
				
				uni.navigateTo({
					url:'../mine/addJobIntention?id='+this.id+'&wantedIntentionId='+this.data.wantedIntention.id
				})
			},
			projecClick(item) {
				uni.navigateTo({
					url: './projectExpere?id=' + this.id + '&projectExpId=' + item.id
				})
			},

			addprojecClick() {
				uni.navigateTo({
					url: './projectExpere?id=' + this.id
				})
			},

			assessmen() {
				uni.navigateTo({
					url: './assessment?id=' + this.id
				})
			}
		}
	}
</script>

<style lang="scss">
	.topView {
		padding: 10px;
		font-size: 12px;
		background-color: #FFFFFF;

		.topview_title {
			font-size: 14px;
		}
	}

	.btxet {
		color: #e8654b;
	}

	.rightBtn {
		width: 12px;
		height: 12px;
	}

	.view_item {
		display: flex;
		height: 25px;
		align-items: center;
		color: #333333;

		.view_item_img {
			width: 10px;
			height: 10px;
		}

		.view_item_text {
			font-size: 12px;
			margin-left: 10px;
			line-height: 25px;
			flex: 1;
		}
	}


	.line {
		margin: 10px 0px 0px 0px;
		height: 1px;
		background-color: #eceff3;
	}

	.plateItemview {
		padding: 10px;

		.plateItemview_titleView {
			display: flex;
			align-items: center;
			height: 30px;

			.plateItemview_titleView_title {
				font-size: 14px;
			}

			.plateItemview_titleView_mes {
				width: 40px;
				height: 15px;
				line-height: 15px;
				font-size: 12px;
				margin-left: 5px;
				color: #e8654b;
				border-radius: 7.5px;
				border: 1px solid #e8654b;
				text-align: center;
			}

			.plateItemview_titleView_noneview {
				flex: 1;
			}
		}


		.down_view {
			display: flex;
			align-items: center;
			justify-content: center;
			padding: 10px;

			.down_view_img {
				width: 15px;
				height: 6px;
			}
		}
	}

	.work_experienceView {
		font-size: 12px;
		margin-top: 5px;

		.work_experienceView_time {
			color: #666666;
			height: 15px;
			line-height: 15px;
			font-size: 12px;

		}

		.work_experienceView_jobview {
			display: flex;
			height: 20px;
			line-height: 20px;
			font-size: 12px;

			.work_experienceView_jobview_title {
				font-size: 12px;
				flex: 1;

			}

			.work_experienceView_jobview_salary {
				font-size: 12px;
			}
		}

		.work_experienceView_detail {
			color: #666666;
			font-size: 12px;
			display: -webkit-box;
			/** 对象作为伸缩盒子模型显示 **/
			overflow: hidden;
			word-break: break-all;
			/* break-all(允许在单词内换行。) */
			text-overflow: ellipsis;
			/* 超出部分省略号 */
			-webkit-box-orient: vertical;
			/** 设置或检索伸缩盒对象的子元素的排列方式 **/
			-webkit-line-clamp: 3;
			/** 显示的行数 **/
		}

	}

	.certificateView {
		display: flex;

		.certificateView_item {
			height: 15px;
			// border-radius: 9.5px;
			// border: 1px solid #e8654b;
			// color: #e8654b;
			margin: 0px 5px 0px 0px;
			line-height: 15px;
			padding: 2px 5px;
			text-align: center;
			font-size: 12px;

		}
	}

	.self_pj {
		color: #666666;
		line-height: 15px;
		font-size: 10px;
	}

	.reload_time {
		text-align: center;
		line-height: 40px;
		font-size: 10px;
		color: #666666;
	}

	.itemview {
		padding: 10px;

		.itemview_title {
			height: 20px;
			line-height: 20px;
			font-size: 14px;
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
</style>
