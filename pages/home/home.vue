<template>
	<view>
		<view class="topView">
			<view class="search">
				<view class="search_view">
					<image class="image_search" src="../../static/home/search.png" mode=""></image>
					<input class="input_v" type="text" v-model="searchValue" value="searchValue" placeholder="输入课程或老师关键字搜索相关课程"
					 confirm-type="search" @confirm="search()" />
				</view>

				<view class="click_sx_view" @click="screenClick()">
					<view class="click_sx_title">筛选</view>
					<image class="click_sx_img" src="../../static/home/sx_o.png" mode=""></image>
				</view>
			</view>
		</view>

		<view class="tab_list">
			<view class="cell" v-for="(item,index) in data" :key="index" @click="itemClick(item)"> 
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
	export default {
		data() {
			return {
				data: [{
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
					}, {
						job: "CRA",
						salary: "10k-15k",
						adress: '北京',
						worktime: '1-3年',
						xl: '本科',
						company: '临语堂（天津）健康管理有限公司',
						releaseTime: '06月12日'
					}, {
						job: "CRA",
						salary: "10k-15k",
						adress: '北京',
						worktime: '1-3年',
						xl: '本科',
						company: '临语堂（天津）健康管理有限公司',
						releaseTime: '06月12日'
					}, {
						job: "CRA",
						salary: "10k-15k",
						adress: '北京',
						worktime: '1-3年',
						xl: '本科',
						company: '临语堂（天津）健康管理有限公司',
						releaseTime: '06月12日'
					},
				]
			};
		},
		
		methods:{
			screenClick(){
				uni.navigateTo({
					url:'./screen'
				})
			},
				
			itemClick(item){
				uni.navigateTo({
					url:'./cvDetail'
				})
			}
			
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
		margin-top: 70px;
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
