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

		<view class="tab_list">
			<view class="cell" v-for="(item,index) in data" :key="index">

				<view class="cell_mainV">

					<view class="cell_content" @click="itemClick(item)">
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

					</view>
				</view>
				<view class="cell_line">

				</view>


			</view>

		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				topArr: ['全部', '待处理', '已接受', '已拒绝'],
				topSelect: '全部',
				data: [{
					job: "CRA",
					salary: "10k-15k",
					adress: '北京',
					worktime: '1-3年',
					xl: '本科',
					company: '临语堂（天津）健康管理有限公司',
					releaseTime: '已接受',
					select: false
				}, {
					job: "CRA",
					salary: "10k-15k",
					adress: '北京',
					worktime: '1-3年',
					xl: '本科',
					company: '临语堂（天津）健康管理有限公司',
					releaseTime: '已拒绝',
					select: true
				}, ]
			};
		},

		methods: {
			screenClick() {
				uni.navigateTo({
					url: './screen'
				})
			},

			itemClick(item) {
				uni.navigateTo({
					url: './cvDetail'
				})
			},
			itemSelect(item) {
				console.log('itemselect');
				item.select = !item.select
			},
			datacommit() {
				console.log("commit");
			},
			topItemClick(item) {
				console.log("item = " + item);
				this.topSelect = item;
			},


		}
	}
</script>

<style lang="scss">
	.topView {
		position: fixed;
		top: 0px;
		width: 100%;
		height: 60px;
		background-color: #f3f3f3;

		.search {
			display: flex;
			width: 100%;
			height: 60px;
			align-items: center;

			.search_view {
				flex: 1;
				display: flex;
				margin: 10px 10px 10px 10px;
				height: 40px;
				background-color: #FFFFFF;
				align-items: center;
				border-radius: 20px;

				.image_search {
					width: 20px;
					height: 20px;
					margin-left: 10px;
				}

				.input_v {
					font-size: 14px;
					color: #666666;
					flex: 1;
					margin-left: 10px;
				}

			}


			.top_fb {
				width: 25px;
				height: 30px;
				margin-right: 15px;
			}
		}
	}

	.click_sx_view {
		display: flex;
		justify-content: center;
		align-items: center;
		margin-right: 15px;

		.click_sx_title {
			font-size: 14px;
			color: #e8654b;
		}

		.click_sx_img {
			width: 10px;
			height: 12px;
			margin-left: 2px;
			margin-top: 2px;
		}
	}

	.tab_list {
		margin-top:40px;
		width: 100%;
		background-color: #FFFFFF;
		line-height: 23px;

		.cell {
			padding: 0px 10px;

			.cell_mainV {
				display: flex;
				// align-items: center;
				padding-bottom: 10px;
			}

			.cell_btn {
				margin: 37px 10px 10px 10px;
				width: 15px;
				height: 15px;
				border: 1px solid #666666;
				background-color: #FFFFFF;

			}

			.cell_btn_select {
				background-color: #e8654b;
				border: 1px solid #e8654b;
			}

			.cell_content {
				flex: 1;
			}

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
				margin: 0px 0px 0px 0px;
				height: 2px;
				background-color: #eceff3;
			}

		}




	}


	.pushBtn {
		position: fixed;
		bottom: 40px;
		right: 0px;
		width: 40px;
		height: 40px;
		background-color: #e8654b;
		border-top-left-radius: 8px;
		border-bottom-left-radius: 8px; //右下角
		color: #FFFFFF;
		font-size: 14px;
		line-height: 20px;
		text-align: center;
		padding: 4px;


	}

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
</style>
