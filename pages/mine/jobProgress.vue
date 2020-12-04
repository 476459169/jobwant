<template>
	<view>

		<view class="top_fixed_view">
			<view class="top_view">
				<view class="top_view_items" v-for="(item,index) in topArr" :key="index">

					<view v-if="index===0" class="top_view_item">
						<image class="top_view_item_img" src="../../static/cv/sx_o.png" mode=""></image>
						<view class="top_view_item_text_select">
							筛选
						</view>
					</view>

					<view class="top_view_item" @click="topItemClick(item)">
						<view :class="[item.content===topSelect.content?'top_view_item_btn_selected':'top_view_item_btn_normal']"></view>
						<view :class="[item.content===topSelect.content?'top_view_item_text_select':'top_view_item_text_normal']">
							{{item.content}}
						</view>
					</view>


				</view>
			</view>
		</view>

		<view class="list">
			<view class="" v-for="(item,index) in data" :key="index">
				<view class="cell" @click="itemselect(item)">

					<view class="cell_endView">
							<image class="cell_endView_img" :src="baseUrl+item.companyHeadImg" mode="">
								<view v-if="item.status==='受邀面试'" class="cell_endView_point"></view>
							</image>
							

					</view>
					<view class="cell_content">
						<view class="cell_top">
							<view class="cell_content_title">{{item.name}}</view>
							<view class="cell_top_salary">
								10K~15K
							</view>
						</view>
						<view class="cell_content_msg">{{item.companyName}}</view>
						<view class="cell_content_bottomview">{{item.resumeStatus}}</view>
					</view>


				</view>

				<view class="line"></view>
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
				page: 1,
				baseUrl:''
			}
		},

		onLoad() {
			_this = this
			this.baseUrl = getApp().globalData.baseUrl
			this.getTopArr()
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

			getTopArr() {
				var loginkey = uni.getStorageSync('loginKey');
				if (loginkey) {
					this.loginKey = loginkey;
					this.$api.post('qzPosition!ajaxGetResumeStatusList.action', {
						loginKey: loginkey
					}).then(res => {
						if (res.res.status == 0) {
							this.topArr = res.inf.arr
						} else {

						}
					})

				} else {}
			},

			getData() {
				var loginkey = uni.getStorageSync('loginKey');
				if (loginkey) {
					this.loginKey = loginkey;
					this.$api.post('qzPosition!ajaxGetJobSearchProgress.action', {
						loginKey: loginkey,
						resumeStatusId: this.topSelect.id ? this.topSelect.id : '',
						page:this.page
					}).then(res => {
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
					})

				} else {}
			},

			topItemClick(item) {
				this.topSelect = item;
				this.page =1
				this.getData()
			},


			itemselect(item) {
				uni.navigateTo({
					url: '../home/cvDetail?id=' + item.positionId+'&resumeStatus='+item.resumeStatus+'&resumeId='+item.resumeId+'&deliveryResumeId='+item.id
				})
			}
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

			.top_view_items {
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
				.cell_top {
					display: flex;

					.cell_top_salary {
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
					border-radius: 9px;
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

				.cell_endView_point {
					margin-left: 40px;
					margin-top: 5px;
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
