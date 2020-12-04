<template>
	<view>
		<view class="top_view">
			<view class="top_view_titleView">
				<view class="top_view_titleView_title">
					{{dataInf.name}}
				</view>
				<view class="top_view_titleView_salary">
					{{dataInf.salaryRequirement}}
				</view>
			</view>
			<view class="top_view_company">
				{{dataInf.companyName}}
			</view>
			<view class="top_view_msgview">
				<image class="top_view_msgview_img" src="../../static/home/jobDetail_time.png" mode=""></image>
				<view class="top_view_msgview_text">{{dataInf.expRequirement}}</view>
				<image class="top_view_msgview_img" src="../../static/home/jobDetail_xl.png" mode=""></image>
				<view class="top_view_msgview_text">{{dataInf.educationRequirement}}</view>
				<image class="top_view_msgview_img" src="../../static/home/jobDetail_status.png" mode=""></image>
				<view class="top_view_msgview_text">{{dataInf.worknature}}</view>
			</view>
			<view v-if="dataInf.welfareArr.length>0">
				<view class="top_view_bottomView">
					<view class="top_view_bottomView_item" v-for="(item,index) in dataInf.welfareArr" :key="index">
						{{item}}
					</view>
				</view>
			</view>
			
		</view>
		
		<view class="title_flexView">
			<image class="title_flexView_img" src="../../static/home/star.png" mode=""></image>
			<view class="title_flexView_text">职位描述</view>
			<view class="title_flexView_line"></view>
		</view>
		<view class="descriptionView">
			{{dataInf.jobRequirement}}
		</view>
		
		<view class="title_flexView">
			<image class="title_flexView_img" src="../../static/home/star.png" mode=""></image>
			<view class="title_flexView_text">公司信息</view>
			<view class="title_flexView_line"></view>
		</view> 
		
		<view class="companyView" @click="companyDetail()">
			<image class="companyView_img" src="../../static/home/star.png" mode=""></image>
			<view class="companyView_contentView">
				<view class="companyView_contentView_title">
					{{dataInf.companyInfo.companyName}}
				</view>
				<view class="companyView_contentView_msg">
					{{dataInf.companyInfo.companyScope+'&nbsp|&nbsp'+dataInf.companyInfo.companyNature+'&nbsp|&nbsp'+dataInf.companyInfo.companyIndustry}}
				</view>
				<view class="companyView_contentView_detail">
					{{dataInf.companyInfo.companyLocation+'&nbsp|&nbsp'+'在招职位'+dataInf.companyInfo.positionCount+'个'}}
				</view>
			</view>
			
			<image class="companyView_jt" src="../../static/home/cerarrow@2x.png" mode=""></image>
		</view>
		
		<view class="warningView">
			<image class="warningView_img" src="../../static/home/warning.png" mode=""></image>
			<view class="warningView_text">
				任何以担保或任何理由索取钱财，扣押证件，均属违法，请您提高警惕！
			</view>
		</view>
		
		<view class="someTJ">
			<view class="someTJ_line"></view>
			<view class="someTJ_text">相似职位推荐</view>
			<view class="someTJ_line"></view>
		</view>
		
		<view class="tab_list">
			<view class="cell" v-for="(item,index) in dataInf.similarPositionArr" :key="index" @click="itemClick(item)"> 
				<view class="cell_titleview">
					<view class="cell_titleview_title">
						{{item.name}}
					</view>
					<view class="cell_titleview_salary">
						{{item.salaryRequirement}}
					</view>
				</view>
				<view class="cell_msg">
					{{item.location+'&nbsp|&nbsp'+item.expRequirement+'&nbsp|&nbsp'+item.educationRequirement}}
				</view>
				<view class="cell_bottomview">
					<view class="cell_bottomview_company">
						{{item.companyName}}
					</view>
					<view class="cell_bottomview_time">
						{{item.upDate}}
					</view>
				</view>
				<view class="cell_line">
		
				</view>
			</view>
		
		</view>
		
		<view class="" style="height: 60px; width: 100%;">
			<view class="bottom_view">
				<!-- <view class="bottom_view_shareBtn">
					分享
				</view> -->
				<view class="bottom_view_pushBtn" @click="pushcv()">
					{{dataInf.isInterviewed ===1?'查看邀请':dataInf.isDelivery===1?'已投递':'投递'}}
				</view>
			</view>
		</view>
		

	
	</view>
</template>

<script>
	export default{
		data(){
				
			return{
				dataInf:{},
				id:'',
				resumeStatus:'',
				resumeId:'',
				deliveryResumeId:'',
				
				
			};
		},
		
		onLoad(e) {
			this.id = e.id
			this.resumeStatus = e.resumeStatus
			this.resumeId = e.resumeId
			this.deliveryResumeId = e.deliveryResumeId
			this.getData()
		},
		methods:{
				
			itemClick(){
				
			},
			
				
			getData(){
				
					var loginkey = uni.getStorageSync('loginKey');
					if (loginkey){
						this.$api.post('qzPosition!ajaxGetPositionDetail.action', {
							loginKey: loginkey,
							positionId:this.id,
							resumeStatus:this.resumeStatus?this.resumeStatus:'',
							resumeId:this.resumeId?this.resumeId:'',
							deliveryResumeId:this.deliveryResumeId?this.deliveryResumeId:'',
							
						}).then(res => {
							if (res.res.status == 0) {
								this.dataInf = res.inf
							} else {
								
							}
						})
						
					}else{
						this.showModal=true;
						this.showLoginModal();
						
					}
					
				 
			},
			
			pushcv(){
				if(this.dataInf.isInterviewed===1){
					//查看面试邀请  sendInvitationCardId
					uni.navigateTo({
						url:'../mine/invitation?id='+this.dataInf.sendInvitationCardId+'&delta=2'
					})
				}
				
				else if(this.dataInf.isDelivery===1){
					uni.showToast({
						title:'请勿重复投递'
					})
				}else{
					var loginkey = uni.getStorageSync('loginKey');
					if (loginkey){
						this.$api.post('qzPosition!ajaxDeliverResume.action', {
							loginKey: loginkey,
							positionId:this.id
						}).then(res => {
							if (res.res.status == 0) {
								
								let pages = getCurrentPages();  //获取所有页面栈实例列表
								let nowPage = pages[ pages.length - 1];  //当前页页面实例
								let prevPage = pages[ pages.length - 2 ];  //上一页页面实例
								for (let i = 0; i <  prevPage.$vm.data.length; i++) {
									let item = prevPage.$vm.data[i]
									if(item.id === this.id){
										item.isDelivery = true
									}
								}
								uni.showToast({
									title:'投递成功',
									success() {
										uni.navigateBack({
											
										})
									}
								})
							} else {
								
							}
						})
						
					}else{
						this.showModal=true;
						this.showLoginModal();
						
					}
				}
				
			},
				
			companyDetail(){
				uni.navigateTo({
					url:'./companyDetail?positionId='+this.id+'&companyId='+this.dataInf.companyInfo.id
				})
			}
		}
	}
</script>

<style lang="scss">
	.top_view{
		padding: 10px;
		margin-top: 10px;
		.top_view_titleView{
			display: flex;
			.top_view_titleView_title{
				font-size: 14px;
				flex: 1;
			}
			.top_view_titleView_salary{
				font-size: 14px;
				color: #e8654b;
			}
		}
		.top_view_company{
			font-size: 12px;
			color: #cccccc;
			line-height: 20px;
			height: 20px;
		}
		
		.top_view_msgview{
			display: flex;
			align-items: center;
			font-size: 12px;
			color: #cccccc;
			height: 20px;
			.top_view_msgview_img{
				width: 10px;
				height: 10px;
			}
			.top_view_msgview_text{
				font-size: 12px;
				color: #cccccc;
				margin-right: 8px;
				margin-left: 3px;
			}
		}
		
		.top_view_bottomView{
			display: flex;
			font-size: 10px;
			flex-wrap: wrap;
			.top_view_bottomView_item{
				padding: 0px 5px;
				color: #e8654b;
				background-color: #fbc9bc;
				margin-right: 5px;
				border-radius: 2px;
				// line-height: 15px;
				font-size: 12px;
				margin-top: 5px;
				
			}
		}
	}
	
	.title_flexView{
		display: flex;
		height:20px;
		align-items: center;
		margin-top: 10px;
		.title_flexView_img{
			height: 15px;
			width: 15px;
			margin: 10px;
		}
		
		.title_flexView_text{
			font-size: 14px;
			height: 20px;
			line-height: 20px;
			
		}
		
		.title_flexView_line{
			margin: 0px 10px;
			height: 1px;
			flex: 1;
			background-color: #e6e6e6;
		}
	}
	
	.descriptionView{
		font-size: 12px;
		padding: 10px;
		line-height: 20px;
		color: #666666;
         white-space: pre-wrap;
	}
	
	.companyView{
		display: flex;
		align-items: center;
		padding: 10px;
		margin: 10px 0px;
		.companyView_img{
			width: 30px;
			height: 30px;
			background-color: #e8654b;
		}
		.companyView_contentView{
			display: flex;
			flex-direction: column;
			justify-content: center;
				margin-left: 10px;
				flex:1;
			.companyView_contentView_title{
				font-size: 14px;
				height: 20px;
				line-height: 20px;
			}
			.companyView_contentView_msg{
				color: #666666;
				font-size: 12px;
				height: 20px;
				line-height: 20px;
			}
			.companyView_contentView_detail{
				color: #666666;
				font-size: 12px;
				height: 20px;
				line-height: 20px;
			}
		}
		
		.companyView_jt{
			width: 5px;
			height: 10px;
		}
		
	}
	
	.warningView{
		
		display: flex;
		padding: 10px;
		align-items: center;
		background-color: #f5f7f8;
		.warningView_img{
			width: 15px;
			height: 15px;
			
		}
		
		.warningView_text{
			font-size: 12px;
			margin-left: 10px;
			color: #333333;
		}
		
	}
	
	.someTJ{
		margin-top: 10px;
		display: flex;
		justify-content: center;
		align-items: center;
		
		.someTJ_line{
			width: 80px;
			height: 1px;
			margin: 0px 10px;
			background-color: #e6e6e6;
		}
		.someTJ_text{
			font-size: 12px;
			color: #666666;
		}
	}
	
	.tab_list {
		margin-top: 10px;
		width: 100%;
		background-color: #FFFFFF;
		line-height: 23px;
	
		.cell {
			padding: 0px 10px;
	
			.cell_titleview {
				display: flex;
				align-items: center;
				margin-top: 10px;
	
				.cell_titleview_title {
					font-size: 14px;
	
					flex: 1;
				}
	
				.cell_titleview_salary {
					font-size: 14px;
	
					color: #e8654b;
				}
	
			}
	
	
			.cell_msg {
				font-size: 12px;
				color: #333333;
	
			}
	
			.cell_bottomview {
				display: flex;
				align-items: center;
	
				.cell_bottomview_company {
	
					flex: 1;
					font-size: 12px;
					color: #333333;
				}
	
				.cell_bottomview_time {
					font-size: 12px;
	
				}
	
			}
	
			.cell_line {
				margin: 10px 0px 0px 0px;
				height: 2px;
				background-color: #eceff3;
			}
	
		}
	
	
	
	
	}
	
	.bottom_view{
		background-color: #FFFFFF;
		height: 60px;
		display: flex;
		position: fixed;
		bottom: 0px;
		align-items: center;
		width: 100%;
		.bottom_view_shareBtn{
			border: 1px solid #e8654b;
			color: #e8654b;
			background-color: #fddfd6;
			height: 40px;
			line-height: 40px;
			font-size: 14px;
			margin-left: 10px;
			margin-right: 10px;
			width: 80px;
			text-align: center;
			border-radius: 5px;
			
		}
		.bottom_view_pushBtn{
			border: 1px solid #e8654b;
			color: #FFFFFF;
			background-color: #e8654b;
			height: 40px;
			line-height: 40px;
			font-size: 14px;
			margin-right: 10px;
			margin-left: 10px;
			flex: 1;
			text-align: center;
			border-radius: 5px;
		}
	}
</style>
