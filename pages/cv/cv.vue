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
				<image class="rightBtn" src="../../static/cv/bj.png" mode=""></image>
			</view>

			<view v-if="data.personMsg.name.length>0">
				<view class="topView_msg">
					{{data.personMsg.age+'&nbsp|&nbsp'+data.personMsg.workTime+'&nbsp|&nbsp'+data.personMsg.address}}
				</view>

				<view class="view_item">
					<image class="view_item_img" src="../../static/cv/phone.png" mode=""></image>
					<view class="view_item_text"> {{data.personMsg.phone}}</view>
				</view>

				<view class="view_item">
					<image class="view_item_img" src="../../static/cv/yx.png" mode=""></image>
					<view class="view_item_text">{{data.personMsg.email}}</view>
				</view>
			</view>
		</view>
		<view class="line"></view>
		
		<view class="mycv">
			<view class="cv_title">我的简历</view>
				
			<view class="cv_item" v-for="(item,index) in data.mycv" :key = "index">
				<view class="cv_item_point"></view>
				<view class="cv_item_text">
					CRA简历
				</view>
				
				<view class="cv_item_settingviews">
					<view class="cv_item_settingviews_item" @click="showcv()">预览</view>
					<view class="cv_item_settingviews_item" >下载</view>
					<view class="cv_item_settingviews_item" @click="seettingCv()">修改</view>
					<view class="cv_item_settingviews_item">删除</view>
				</view>
			</view>
			
			<view class="line"></view>
		</view>
		
		<view class="mycv">
			<view class="cv_title">导入简历</view>	
			<view class="cv_item">
				<view class="cv_item_point">
					
				</view>
				<view class="cv_item_text">
					CRA简历
				</view>
				
				<view class="cv_item_settingviews">
					<view class="cv_item_settingviews_item">预览</view>
					<view class="cv_item_settingviews_item">删除</view>
				</view>
			</view>
			<view class="line"></view>
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

					mycv: [{
						cvName: 'CRA简历',
						cvid: ''
					}, {
						cvName: 'CRA简历',
						cvid: ''
					}],
					
					

					evaluation: '自我评价 自我评价 自我评价 自我评价 自我评价 自我评价',
					reloadTime: "2020.04.26"



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


			wantjobClick() {
				uni.navigateTo({
					url: '../mine/jobManage'
				})
			},
			
			showcv(){
				uni.navigateTo({
					url:'./showcv'
				})
			},
			seettingCv(){
				uni.navigateTo({
					url:'./cvsetting'
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

	.rightBtn {
		width: 12px;
		height: 12px;
	}



	.line {
		margin: 10px 0px 0px 0px;
		height: 1px;
		background-color: #eceff3;
	}
	
	.mycv{
		padding: 20px 10px 0px 10px;
		.cv_title{
			font-size: 12px;
			color: #666666;
			margin-bottom: 10px;
		}
		
		.cv_item{
			display: flex;
			align-items: center;
			margin: 5px 0px 0px 0px;
			.cv_item_point{
				width: 5px;
				height: 5px;
				border-radius: 2.5px;
				background-color: #e8654b;
			}
			.cv_item_text{
				font-size: 12px;
				margin-left: 10px;
				flex: 1;
			}
			.cv_item_settingviews{
				display: flex;
				align-items: center;
				justify-content: center;
			
				.cv_item_settingviews_item{
					padding: 5px;
					color: #e8654b;
					font-size: 12px;
				}
			}
			
		}
	}

</style>
