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
					
					<view class="top_view_item" @click="topItemClick(item)">
						<view  :class="[item.content===topSelect.content?'top_view_item_btn_selected':'top_view_item_btn_normal']"></view>
						<view  :class="[item.content===topSelect.content?'top_view_item_text_select':'top_view_item_text_normal']">
							{{item.content}}
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
								{{item.positionName}}
							</view>
							<view class="cell_titleview_salary">
								{{item.salaryRequirement}}
							</view>
						</view>
						<view class="cell_msg">
							{{item.workLocation+'&nbsp|&nbsp'+item.experienceRequirement+'&nbsp|&nbsp'+item.educationRequirement}}
						</view>
						<view class="cell_bottomview">
							<view class="cell_bottomview_company">
								{{item.companyName}}
							</view>
							<view class="cell_bottomview_time">
								{{item.status}}
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
	
	var _this;
	var timer = null;
	export default {
		data() {
			return {
				topArr: [],
				topSelect: Object,
				data: [],
				page:1
			};
		},
		
		onLoad() {
			_this = this
				this.getTopArr()
				
		},
		
		onShow() {
			this.page = 1;
			this.getData()
		},
		
		onPullDownRefresh: function() {
			this.page = 1;
			this.getData()
		
		},
		
		onReachBottom: function() { //当划到最底部的时候触发事件
			if (timer != null) { //加载缓冲延迟
				clearTimeout(timer);
			}
			timer = setTimeout(function() {
				_this.getData();
			}, 600);
		},

		methods: {
			
			
			getTopArr(){
				var loginkey = uni.getStorageSync('loginKey');
				if (loginkey){
					this.loginKey = loginkey;
					this.$api.post('qzPosition!ajaxGetInterviewInvitationStatusStatusList.action', {
						loginKey: loginkey
					}).then(res => {
						if (res.res.status == 0) {
							this.topArr = res.inf.arr
						} else {
							
						}
					})
					
				}else{
				}
			},
			
			getData(){
				var loginkey = uni.getStorageSync('loginKey');
				if (loginkey){
					this.loginKey = loginkey;
					this.$api.post('qzPosition!ajaxGetInterviewInvitationList.action', {
						loginKey: loginkey,
						interviewInvitationStatus:this.topSelect.id?this.topSelect.id:this.topSelect.id===0?'0':'',
						first:this.page
					}).then(res => {
						if (res.res.status == 0) {
							if (res.res.status == 0) {
								if (_this.page === 1) {
									this.data = res.inf.arr
									_this.page++;
								} else {
									if (_this.page <= res.inf.pageCount) {
										_this.data = _this.data.concat(res.inf.arr); //进行数据的累加
										_this.page++; //页数的++
										_this.loading = "加载更多";
									} else {
										uni.showToast({
											title: '没有更多了！'
										});
									}
								}
							
							} else {
								uni.showToast({
									title: res.res.error
								});
							}
							
							uni.hideNavigationBarLoading();
							uni.stopPullDownRefresh(); //数据加载完成,刷新结束
						} else {
						}
					})
					
				}else{
					
				}
			},
			screenClick() {
				uni.navigateTo({
					url: './screen'
				})
			},

			itemClick(item) {
				uni.navigateTo({
					url: './invitation?id='+item.id+'&delta=1'
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
				console.log('index = '+item.id);
				this.topSelect = item;
				this.page = 1
				this.getData()
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
