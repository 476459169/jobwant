<template>
	<view>
		<view class="top_view">
			<view class="top_view_titleView">
				<view class="top_view_titleView_title">
					{{data.job}}
				</view>
				<view class="top_view_titleView_salary">
					{{data.salary}}
				</view>
			</view>
			<view class="top_view_company">
				{{data.company}}
			</view>
			<view class="top_view_msgview">
				<image class="top_view_msgview_img" src="../../static/cv/sj.png" mode=""></image>
				<view class="top_view_msgview_text">{{data.workTime}}</view>
				<image class="top_view_msgview_img" src="../../static/cv/sj.png" mode=""></image>
				<view class="top_view_msgview_text">{{data.xl}}</view>
				<image class="top_view_msgview_img" src="../../static/cv/sj.png" mode=""></image>
				<view class="top_view_msgview_text">{{data.status}}</view>
			</view>
			<view class="top_view_bottomView">
				<view class="top_view_bottomView_item" v-for="(item,index) in data.welfare" :key="index">
					{{item}}
				</view>
			</view>
		</view>
		
		<view class="title_flexView">
			<image class="title_flexView_img" src="../../static/home/star.png" mode=""></image>
			<view class="title_flexView_text">职位描述</view>
			<view class="title_flexView_line"></view>
		</view>
		<view class="descriptionView">
			{{data.description}}
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
					{{data.companyMsg.company}}
				</view>
				<view class="companyView_contentView_msg">
					{{data.companyMsg.peopleNumber+'&nbsp|&nbsp'+data.companyMsg.type+'&nbsp|&nbsp'+data.companyMsg.lb}}
				</view>
				<view class="companyView_contentView_detail">
					{{data.companyMsg.address+'&nbsp|&nbsp'+data.companyMsg.mes}}
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
			<view class="cell" v-for="(item,index) in data.list" :key="index" @click="itemClick(item)"> 
				<view class="cell_titleview">
					<view class="cell_titleview_title">
						{{item.job}}
					</view>
					<view class="cell_titleview_salary">
						{{item.salary}}
					</view>
				</view>
				<view class="cell_msg">
					{{item.adress+'&nbsp|&nbsp'+item.worktime+'&nbsp|&nbsp'+item.xl}}
				</view>
				<view class="cell_bottomview">
					<view class="cell_bottomview_company">
						{{item.company}}
					</view>
					<view class="cell_bottomview_time">
						{{item.releaseTime}}
					</view>
				</view>
				<view class="cell_line">
		
				</view>
			</view>
		
		</view>
	</view>
</template>

<script>
	export default{
		data(){
				
			return{
				data:{
					job:'CRA',
					salary:'10k-15k/月',
					company:'临语堂（天津）健康管理有限公司',
					xl:'本科',
					status:'全职',
					workTime:'1-3年',
					welfare:['五险一金','年底双薪','全勤奖'],
					description:'1. 可以通过微信开发者工具切换pages.json中condition配置的页面，或者关闭微信开发者工具，然后再从HBuilderX中启动指定页面\n2. 如果出现微信开发者工具启动后白屏的问题，检查是否启动多个微信开发者工具，如果是则关闭所有打开的微信开发者工具，然后再重新运行',
					companyMsg:{
						company:'临语堂（天津）健康管理有限公司',
						peopleNumber:'100-299人',
						type:'民营',
						lb:'互联网/医药',
						address:'北京',
						mes:'在招职位15个'
					},
					list:[
						{
								job: "CRA",
								salary: "10k-15k",
								adress: '北京',
								worktime: '1-3年',
								xl: '本科',
								company: '临语堂（天津）健康管理有限公司',
								releaseTime: '06月12日'
							},
							{
								job: "CRA",
								salary: "10k-15k",
								adress: '北京',
								worktime: '1-3年',
								xl: '本科',
								company: '临语堂（天津）健康管理有限公司',
								releaseTime: '06月12日'
							},
					]
				}
			};
		},
		methods:{
				
			itemClick(){
			},
			
			
				
			companyDetail(){
				uni.navigateTo({
					url:'./companyDetail'
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
				margin-right: 5px;
			}
		}
		
		.top_view_bottomView{
			margin-top: 5px;
			display: flex;
			height: 15px;
			font-size: 10px;
			.top_view_bottomView_item{
				padding: 0px 4px;
				color: #e8654b;
				background-color: #fbc9bc;
				margin-right: 5px;
				border-radius: 2px;
				line-height: 15px;
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
</style>
