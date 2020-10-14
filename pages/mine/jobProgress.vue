<template>
	<view>

		<view class="top_fixed_view">
			<view class="top_view">
				<view  class="top_view_items" v-for="(item,index) in topArr" :key="index" >
					
					<view v-if="index===0" class="top_view_item">
						<image  class="top_view_item_img" src="../../static/cv/sx_o.png" mode=""></image>
						<view   class="top_view_item_text_select">
							筛选
						</view>
					</view>
					
					<view v-else class="top_view_item" @click="topItemClick(item)">
						<view  :class="[item===topSelect?'top_view_item_btn_selected':'top_view_item_btn_normal']"></view>
						<view  :class="[item===topSelect?'top_view_item_text_select':'top_view_item_text_normal']">
							{{item}}
						</view>
					</view>
					
					
				</view>
			</view>
		</view>

		<view class="list">
			<view class="" v-for="(item,index) in data" :key="index">
				<view class="cell">

					<view class="cell_endView">
						<view class="cell_endView_img">
							<view v-if="item.status==='受邀面试'" class="cell_endView_point"></view>
						</view>
						
					</view>
					<view class="cell_content">
						<view class="cell_top">
							<view class="cell_content_title">{{item.company}}</view>
							<view class="cell_top_salary">
								10K~15K
							</view>
						</view>
						<view class="cell_content_msg">{{item.progress}}</view>
						<view class="cell_content_bottomview">{{item.status}}</view>
					</view>


				</view>

				<view class="line"></view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				topArr: ['全部','已投递', '已查看', '受邀面试', '不合适'],
				topSelect: '全部',
				data: [{
						company: 'CRA',
						progress: '临语堂（天津）健康管理有限公司',
						status: '受邀面试'
					},
					{
						company: '临语堂（天津）健康管理有限公司',
						progress: '感谢您的投递，我会尽快查看您的简历感谢您的投递，我会尽快查看您的简历感谢您的投递，我会尽快查看您的简历',
						status: '已投递'
					},
					{
						company: '临语堂（天津）健康管理有限公司',
						progress: '感谢您的投递，我会尽快查看您的简历感谢您的投递，我会尽快查看您的简历感谢您的投递，我会尽快查看您的简历',
						status: '已投递'
					},
				]

			}
		},
		methods: {

			topItemClick(item) {
				console.log("item = " + item);
				this.topSelect = item;
			},
		}
	}
</script>

<style lang="scss">
	.top_fixed_view {
		position: fixed;
		top: 0px;
		width: 100%;
		padding: 0px 0px;
		background-color: #e7e9ea;
	
	
	
	
		.top_view {
			position: relative;
			display: flex;
			// flex-wrap: wrap;
	
			.top_view_items{
				display: flex;
				align-items: center;
			}
	
			.top_view_item {
				display: flex;
				align-items: center;
				padding: 10px 5px;
				margin-left: 5px;
	
				.top_view_item_img {
					width: 10px;
					height: 10px;
				}
	
				.top_view_item_btn_normal {
					width: 8px;
					height: 8px;
					border: 1px solid #000000;
					background-color: #e7e9ea;
					border-radius: 4px;
					
				}
	
				.top_view_item_btn_selected {
					width: 8px;
					height: 8px;
					border: 1px solid #e8654b;
					border-radius: 4px;
					// background-color: #e8654b;
	
				}
	
				.top_view_item_text_normal {
					font-size: 14px;
					color: #666666;
					margin-left: 2px;
				}
	
				.top_view_item_text_select {
					font-size: 14px;
					color: #e8654b;
					margin-left: 2px;
				}
			}
	
			.top_view_select_color {
				color: #e8654b;
			}
		}
	}


	.list {
		margin-top: 40px;

		.cell {
			display: flex;
			align-items: center;
			padding: 10px;

			.cell_content {
				display: flex;
				flex-direction: column;
				justify-content: center;
				flex: 1;
				// margin-left: 20px;
				.cell_top{
					display: flex;
					.cell_top_salary{
						margin-right: 10px;
						font-size: 14px;
						color: #e8654b;
						line-height: 25px;
						
					}
				}

				.cell_content_title {
					font-size: 14px;
					line-height: 25px;
					flex: 1;
				}

				.cell_content_msg {
					font-size: 12px;
					line-height: 25px;
					
					color: #666666;
					width: calc(100vw - 80px);
					white-space: nowrap;
					overflow: hidden;
					text-overflow: ellipsis;
				}

				.cell_content_bottomview {
					font-size: 10px;
					line-height: 15px;
					height: 15px;
					width: 60px;
					color: #666666;
					text-align: center;
					border: 1px solid #666666;
					border-radius:9px;
					margin: 5px 0px;
					padding: 1.5px;
				}
			}

			.cell_endView {
				display: flex;
				// flex-direction: column;
				// justify-content: center;
				// align-items: center;

				.cell_endView_img {
					margin: 10px;
					width: 50px;
					height: 50px;
					border-radius: 25px;
					background-color: #e8654b
				}

				.cell_endView_btn {
					margin-top: 10px;
					border: 1px solid #666666;
					border-radius: 10px;
					height: 20px;
					width: 80px;
					font-size: 12px;
					line-height: 20px;
					text-align: center;
				}
				.cell_endView_point{
					margin-left: 40px;
					margin-top:5px;
					background-color: #FF0000;
					width: 10px;
					height: 10px;
					border-radius: 5px;
				}
			}
		}
	}

	.line {
		margin: 0px 10px;
		height: 1px;
		background-color: #f5f7f8;

	}
</style>
