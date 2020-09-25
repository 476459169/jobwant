<template>
	<view >
		<view class="main_cs">
			<view class="topView">
				<view class="topView_title">
					临语堂（天津）健康管理有限公司
				</view>
				<view class="topView_item">
					<image class="topView_item_img" src="../../static/cv/b.png" mode=""></image>
					<view class="topView_item_text">互联网/培训</view>
				</view>
				<view class="topView_item">
					<image class="topView_item_img" src="../../static/cv/b.png" mode=""></image>
					<view class="topView_item_text">股份制企业</view>
				</view>
				<view class="topView_item">
					<image class="topView_item_img" src="../../static/cv/b.png" mode=""></image>
					<view class="topView_item_text">20人以下</view>
				</view>
			</view>
			
			<view class="title_flexView">
				<image class="title_flexView_img" src="../../static/home/star.png" mode=""></image>
				<view class="title_flexView_text">公司详情</view>
				<view class="title_flexView_line"></view>
			</view> 
			
			<view class="descriptionView">
				{{dataArr.companyDetail}}
			</view>
		</view>
		
		<view class="lineView">
			- 他们还在招 -
		</view>
		
		<view class="tab_list">
			<view class="cell" v-for="(item,index) in dataArr.listArr" :key="index" @click="itemClick(item)"> 
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
				dataArr:{
					company:'临语堂（天津）健康管理有限公司',
					type:'互联网/培训',
					status:'股份制企业',
					peopleNumber:'20人以下',
					companyDetail:'Vue.js（读音 /vjuː/, 类似于 view） 是一套构建用户界面的渐进式框架Vue 只关注视图层， 采用自底向上增量开发的设计。Vue 的目标是通过尽可能简单的 API 实现响应的数据绑定和组合的视图组件。Vue 学习起来非常简单，本教程基于 Vue 2.1.8 版本测试。',
					listArr:[
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
							}
					]
				}
			};
		}
	}
</script>

<style lang="scss">
	.main_cs{
		
	}
	
	.topView{
		padding: 10px;
		.topView_title{
			font-size: 14px;
			height: 30px;
			line-height: 30px;
		}
		
		.topView_item{
			display: flex;
			align-items: center;
			height: 25px;
			.topView_item_img{
				width: 10px;
				height: 10px;
				background-color: #007AFF;
			}
			.topView_item_text{
				height: 20px;
				line-height: 20px;
				font-size: 12px;
				margin-left: 10px;
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
	
		
	.lineView{
		width: 100%;
		height: 40px;
		text-align: center;
		line-height: 40px;
		color: #333333;
		background-color: #f5f7f8;
		font-size: 12px;
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
