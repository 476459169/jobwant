<template>
	<view>
		<view class="topView">

			<view class="topView_nameView">
				<view class="topView_nameView_name">
					{{data.personMsg.name.length>0?data.personMsg.name:'姓名'}}
				</view>
				<view class="topView_nameView_xb">
					{{data.personMsg.gender.length>0?data.personMsg.gender:'/女士'}}
				</view>
				<!-- <image class="rightBtn" src="../../static/cv/bj.png" mode=""></image> -->
			</view>

			<view v-if="data.personMsg.name.length>0">
				<view class="topView_msg">
					{{data.personMsg.age+'&nbsp|&nbsp'+data.personMsg.workTime+'&nbsp|&nbsp'+data.personMsg.address}}
				</view>

				<view class="view_item">
					<image class="view_item_img" src="../../static/cv/phone.png" mode=""></image>
					<view class="view_item_text">	{{data.personMsg.phone}}</view>
				</view>

				<view class="view_item">
					<image class="view_item_img" src="../../static/cv/yx.png" mode=""></image>
					<view class="view_item_text">{{data.personMsg.email}}</view>
				</view>
			</view>

			<view class="line"></view>
		</view>

		<view class="plateItemview">
			<view class="plateItemview_titleView" @click="wantjobClick()">
				<view class="plateItemview_titleView_title">求职意向</view>
				<!-- <view class="plateItemview_titleView_mes">必填</view> -->
				<view class="plateItemview_titleView_noneview"></view>
				<!-- <image class="rightBtn" src="../../static/cv/sz.png" mode=""></image> -->
			</view>

			<view class="" v-if="data.jobwant.time.length>0">
				<view class="view_item">
					<image class="view_item_img" src="../../static/cv/sj.png" mode=""></image>
					<view class="view_item_text">{{data.jobwant.time}}</view>
				</view>
				<view class="" v-for="(item,index) in data.jobwant.job" :key="index">
					<view class="view_item">
						<image class="view_item_img" src="../../static/cv/b.png" mode=""></image>
						<view class="view_item_text">{{item}}</view>
					</view>
				</view>
			</view>
		</view>

		<view class="plateItemview">
			<view class="plateItemview_titleView">
				<view class="plateItemview_titleView_title">
					工作/实习经验
				</view>
				<!-- <view class="plateItemview_titleView_mes">
					必填
				</view> -->
				<view class="plateItemview_titleView_noneview"></view>
				<!-- <image  class="rightBtn" src="../../static/cv/tj.png" mode=""></image> -->
			</view>

			<view v-if="data.workExperience.length>0">
				<view class="work_experienceView" v-for="(item,index) in data.workExperience" :key="index">
					<view class="" v-if="index<showWorkExperienceNumb">
						<view class="view_item">
							<image class="view_item_img" src="../../static/cv/gs.png"></image>
							<view class="view_item_text">{{item.company}}</view>
							<!-- <image  class="rightBtn" src="../../static/cv/bj.png" mode=""></image> -->
						</view>
						<view class="work_experienceView_time">{{item.beginTime+'-'+item.endTime}}</view>
						<view class="work_experienceView_jobview">
							<view class="work_experienceView_jobview_title">{{item.job}}</view>
							<view class="work_experienceView_jobview_salary">{{item.salary}}</view>
						</view>
						<view class="work_experienceView_detail">{{item.detail}}</view>
					</view>
				</view>
			</view>

			<view class="down_view" @click="dowmViewClick()">
				<image  v-if="showWorkExperience==true"  class="down_view_img" src="../../static/cv/upcrrow.png" mode=""></image>
				<image v-else  class="down_view_img" src="../../static/cv/downcrrow.png" mode=""></image>
				
			</view>
		</view>

		<view class="plateItemview">
			<view class="plateItemview_titleView">
				<view class="plateItemview_titleView_title">教育经历</view>
				<!-- <view class="plateItemview_titleView_mes">必填</view> -->
				<view class="plateItemview_titleView_noneview"></view>
				<!-- <image class="rightBtn" src="../../static/cv/tj.png"></image> -->
			</view>

			<view v-if="data.educationExperience.length>0">
				<view class="work_experienceView" v-for="(item,index) in data.educationExperience" :key="index">
					<view class="view_item">
						<image class="view_item_img" src="../../static/cv/b.png"></image>
						<view class="view_item_text">{{item.school}}</view>
						<!-- <image class="rightBtn" src="../../static/cv/bj.png"> </image> -->
					</view>
					<view class="work_experienceView_time">{{item.beginTime+'-'+item.endTime}}</view>
					<view class="work_experienceView_jobview">
						<view class="work_experienceView_jobview_title">
							{{item.xl+'&nbsp|&nbsp'+item.discipline}}
						</view>
					</view>
				</view>
			</view>
		</view>

		<view class="plateItemview">
			<view class="plateItemview_titleView">
				<view class="plateItemview_titleView_title">项目经历</view>
				<view class="plateItemview_titleView_noneview"></view>
				<!-- <image class="rightBtn" src="../../static/cv/tj.png"></image> -->
			</view>
			<view v-if="data.projectExperience.length>0">
				<view class="work_experienceView" v-for="(item,index) in data.projectExperience" :key="index">
					<view class="view_item">
						<image class="view_item_img" src="../../static/cv/b.png"></image>
						<view class="view_item_text">{{item.project}}</view>
						<!-- <image class="rightBtn" src="../../static/cv/bj.png"></image> -->
					</view>
					<view class="work_experienceView_time"> {{item.beginTime+'-'+item.endTime}}</view>
					<view class="work_experienceView_jobview">
						<view class="work_experienceView_jobview_title" style="color: #666666;">
							{{item.msg}}
						</view>
					</view>
				</view>
			</view>
		</view>

		<view class="plateItemview">
			<view class="plateItemview_titleView">
				<view class="plateItemview_titleView_title">获得证书</view>
				<view class="plateItemview_titleView_noneview"></view>
				<!-- <image class="rightBtn" src="../../static/cv/tj.png"></image> -->
			</view>
			<view v-if="data.certificate.length>0">
				<view v-for="(item,index) in data.certificate" :key="index">
					<view class="view_item">
						<image class="view_item_img" src="../../static/cv/b.png"></image>
						<view class="view_item_text">{{item.certificateName}}</view>
					</view>
				</view>
			</view>
		</view>
		
		
		<view class="plateItemview">
			<view class="plateItemview_titleView">
				<view class="plateItemview_titleView_title">培训经历</view>
				<view class="plateItemview_titleView_noneview"></view>
				<!-- <image class="rightBtn" src="../../static/cv/tj.png"></image> -->
			</view>
			<view v-if="data.trainingExperience.length>0">
				<view class="work_experienceView" v-for="(item,index) in data.trainingExperience" :key="index">
					<view class="view_item">
						<image class="view_item_img" src="../../static/cv/gs.png"></image>
						<view class="view_item_text">{{item.trainingJG}}</view>
						<!-- <image class="rightBtn" src="../../static/cv/bj.png"></image> -->
					</view>
					<view class="work_experienceView_time"> {{item.beginTime+'-'+item.endTime}}</view>
				</view>
			</view>
		</view>
		
		<view class="plateItemview">
			<view class="plateItemview_titleView">
				<view class="plateItemview_titleView_title">语言能力</view>
				<view class="plateItemview_titleView_noneview"></view>
				<!-- <image class="rightBtn" src="../../static/cv/tj.png"></image> -->
			</view>
			<view v-if="data.language.length>0">
				<view v-for="(item,index) in data.language" :key="index">
					<view class="view_item">
						<image class="view_item_img" src="../../static/cv/b.png"></image>
						<view class="view_item_text">{{item.language}}</view>
					</view>
				</view>
			</view>
		</view>
		
		
		<view class="plateItemview">
			<view class="plateItemview_titleView">
				<view class="plateItemview_titleView_title">自我评价</view>
				<view class="plateItemview_titleView_noneview"></view>
				<!-- <image class="rightBtn" src="../../static/cv/bj.png"></image> -->
			</view>
			<view class="self_pj">
				{{data.evaluation}}
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
				data: {
					personMsg: {
						name: '临小妹',
						age: '36岁',
						gender: '女士',
						workTime: '工作17年',
						address: '北京-朝阳',
						phone: '13552300611',
						email: '476459169@qq.com',
						xl: '本科',

					},
					jobwant: {
						time: '月内到岗',
						job: ['CRA', 'PM'],

					},
					workExperience: [{
							company: '临语堂（天津）健康管理有限公司',
							beginTime: '2018.3',
							endTime: '至今',
							job: 'CTA',
							detail: '工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情',
							salary: '7k/月'
						},
						{
							company: '临语堂（天津）健康管理有限公司',
							beginTime: '2018.3',
							endTime: '至今',
							job: 'CTA',
							detail: '工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情',
							salary: '7k/月'
						},
						{
							company: '临语堂（天津）健康管理有限公司',
							beginTime: '2018.3',
							endTime: '至今',
							job: 'CTA',
							detail: '工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情',
							salary: '7k/月'
						}
					],
					educationExperience: [{
							school: '湖北医药大学',
							beginTime: '2018.3',
							endTime: '至今',
							xl: '本科',
							discipline: '药理学'
						},
						{
							school: '湖北医药大学',
							beginTime: '2018.3',
							endTime: '至今',
							xl: '本科',
							discipline: '药理学'
						}
					],
					projectExperience: [{
						project: '肿瘤项目三期',
						beginTime: '2018.3',
						endTime: '至今',
						msg: '工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情工作详情工'
					}],
					certificate: [{
							certificateName: 'GCP证书',
							certificateJG: '临语堂'
						},
						{
							certificateName: 'GCP证书',
							certificateJG: '临语堂'
						}
					],
					trainingExperience: [{
							trainingJG: '临语堂培训',
							beginTime: '2018.3',
							endTime: '至今',
						},
						{
							trainingJG: '临语堂培训',
							beginTime: '2018.3',
							endTime: '至今',
						}
					],
					language: [{
							language: '汉语'
						},
						{
							language: '英语'
						}
					],
					evaluation: '自我评价 自我评价 自我评价 自我评价 自我评价 自我评价',
					reloadTime:"2020.04.26"



				}
			};
		},
		methods: {
			dowmViewClick() {
				this.showWorkExperience = !this.showWorkExperience
				if (this.showWorkExperience == true) {
					this.showWorkExperienceNumb = this.data.workExperience.length
				} else {
					this.showWorkExperienceNumb = 2
				}
				console.log('this.showWorkExperience = ' + this.showWorkExperienceNumb);
			},
			
				
			wantjobClick(){
				uni.navigateTo({
					url:'../mine/jobManage'
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

		.topView_nameView {
			display: flex;


			.topView_nameView_name {
				font-size: 16px;
			}

			.topView_nameView_xb {
				color: #999999;
				font-size: 12px;
				margin-top: 3px;
				margin-left: 3px;
				flex: 1;
			}
		}

		.topView_msg {
			color: #333333;
			height: 25px;
			line-height: 25px;

		}

		.topView_item {
			display: flex;
			height: 25px;
			align-items: center;

			.topView_item_img {
				width: 10px;
				height: 10px;
				background-color: #000000;
			}

			.topView_item_text {
				font-size: 12px;
				margin-left: 10px;
				line-height: 25px;
				flex: 1;
			}
		}

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
			padding: 10px ;
			.down_view_img {
				width: 15px;
				height: 6px;
			}
		}
	}

	.work_experienceView {
		font-size: 12px;

		.work_experienceView_time {
			color: #666666;
			height: 15px;
			line-height: 15px;

		}

		.work_experienceView_jobview {
			display: flex;
			height: 20px;
			line-height: 20px;

			.work_experienceView_jobview_title {
				flex: 1;
			}

			.work_experienceView_jobview_salary {}
		}

		.work_experienceView_detail {
			color: #666666;
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
	
	.self_pj{
		color: #666666;
		line-height: 15px;
		font-size: 10px;
	}
	
	.reload_time{
		text-align: center;
		line-height: 40px;
		font-size: 10px;
		color: #666666;
	}
</style>
