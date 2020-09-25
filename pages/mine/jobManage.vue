<template>
	<view>
		<view class="top_view">
			<view class="top_view_title">
				管理求职意向 <text class="top_view_ftitle">( <text style="color: #e8654b;">3</text>/3) </text>
			</view>
			<view class="top_view_msg">
				我们会根据你的"求职意向"在首页展示合适的职位
			</view>
		</view>


		<view class="selectView" @tap="handleTap('picker31')">
			<view class="titleLabel">求职状态</view>
			<view class="selectView_btn">{{type}}{{zwf}}⟩</view>
		</view>
		
	<!-- 	<view class="company" >
			<view class="company_content">{{type}}</view>
			<view class="company_imgView">
				<image class="company_img" src="../../static/login/star3.png" mode=""></image>
			</view>
		</view> -->
		<view class="line">	</view>
		<lb-picker ref="picker31"
		  v-model="type"
		  mode="selector"
		  :list="typelist">
		  </lb-picker>


		<view class="list">
			<view v-for="(item,index) in dataArr" :key="index">
				<view class="cell">
					<view class="cell_content">
						<view class="cell_content_title">
							{{item.jobName}}
						</view>
						<view class="cell_content_detail">
							{{item.salary+'&nbsp|&nbsp'+item.address+'&nbsp|&nbsp'+item.status}}
						</view>
					</view>
					<image class="cell_endBtn" src="../../static/cv/bj.png" mode=""></image>
				</view>
				<view class="line">
					
				</view>
			</view>
		</view>
		
		
		<view class="bottom_view" >
			<view class="bottom_btn" @click="addClick()">
				添加求职意向
			</view>
		</view>

	</view>
</template>

<script>
	export default {

		data() {
			return {
				typelist: ['离职，随时到岗', '在职，月内到岗', '在职，考虑机会','在职，暂不考虑'],
				type: '在职，暂不考虑',
				zwf:'\u0020',
				dataArr:[
					{
						jobName:'CRA',
						salary:'10k-15k',
						address:'北京',
						status:'全职'
					},
					{
						jobName:'CRA',
						salary:'10k-15k',
						address:'北京',
						status:'全职'
					}
				]
			};
		},

		methods: {
			handleTap(picker) {
				this.$refs[picker].show()
			},
			
			addClick(){
				uni.navigateTo({
					url:'./addJobIntention'
				})
			}
		}
	}
</script>

<style lang="scss">
	.top_view {
		padding: 10px 10px;
		margin-top: 10px;

		.top_view_title {
			font-size: 14px;
		}

		.top_view_ftitle {
			font-size: 12px;
			color: #666666;
		}

		.top_view_msg {
			font-size: 12px;
			height: 30px;
			line-height: 30px;
			color: #666666;
		}

	}

	.company {
		display: flex;
		margin: 0px 10px 10px 10px;
		border-radius: 5px;
		background-color: #f5f7f8;
		font-size: 14px;
		color: #666666;

		.company_content {
			flex: 1;
			padding: 5px;

		}

		.company_imgView {
			width: 30px;
			height: 30px;
			background-color: #e8654b;
			border-top-right-radius: 5px;
			border-bottom-right-radius: 5px; //右下角

			display: flex;
			align-items: center;
			justify-content: center;

			.company_img {
				width: 8px;
				height: 5px;
			}
		}

	}

	.titleLabel {
		font-size: 14px;
		padding: 10px 10px;
		line-height: 20px;
		flex: 1;
	}
	
	.list{
		padding: 10px;
		
		.cell{
			display: flex;
			align-items: center;
			.cell_content{
				flex:1;
				.cell_content_title{
					font-size: 14px;
					height: 30px;
					line-height: 30px;
				}
				
				.cell_content_detail{
					font-size: 12px;
					height: 30px;
					line-height: 30px;
				}
			}
			.cell_endBtn{
				width: 20px;
				height: 20px;
			}
			
		}
	}
	
	.line{
		margin: 0px;
		width: 100%;
		height: 1px;
		background-color: #e6e6e6;
	}
	
	.bottom_view{
		width: 100%;
		height: 60px;
		display: flex;
		justify-content: center;
		align-items: center;
		position: fixed;
		bottom: 0;
		.bottom_btn{
		
			width: 80%;
			height: 40px;
			background-color: #e8654b;
			font-size: 14px;
			color: #FFFFFF;
			line-height: 40px;
			text-align: center;
			border-radius: 5px;
		}
	}
	
	.selectView{
		display: flex;
		align-items: center;
		margin-bottom: 10px;
	}
	
	.selectView_btn{
		font-size: 12px;
		color: #666666;
		margin-right: 10px;
	}
	
</style>
