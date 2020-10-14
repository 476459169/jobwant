<template>
	<view>
		<d-alert v-if="showModal"></d-alert>
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

		<view class="tab_list" >
			<view class="cell" v-for="(item,index) in data" :key="index">

				<view class="cell_mainV">
					<view class="cell_btn" :class="[item.select==true?'cell_btn_select':'']" @click="itemSelect(item)">

					</view>

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


		<view class="pushBtn" @click="datacommit()">
			批量\n投递
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				loginkey:'',
				showModal:true,
				data: [{
					job: "CRA",
					salary: "10k-15k",
					adress: '北京',
					worktime: '1-3年',
					xl: '本科',
					company: '临语堂（天津）健康管理有限公司',
					releaseTime: '06月12日',
					select: false
				}, {
					job: "CRA",
					salary: "10k-15k",
					adress: '北京',
					worktime: '1-3年',
					xl: '本科',
					company: '临语堂（天津）健康管理有限公司',
					releaseTime: '06月12日',
					select: true
				}, ]
			};
		},

		onLoad() {
			// if(!this.loginkey){
			// 	this.datacommit();
			// }

		},

		onShow() {
			this.getuserInfo();
		},

		methods: {
			getuserInfo() {
				// 
				var loginkey = uni.getStorageSync('loginKey');
				if (loginkey){
					this.$api.post('user!ajaxGetUserInfo.action', {
						loginKey: loginkey
					}).then(res => {
						if (res.res.status == 0) {
							// this.sfz = res.inf.subCardNo
							console.log("have loginkey");
							this.showModal=false;
							
						} else {
							uni.removeStorageSync('loginKey');
							uni.removeStorageSync('userId');
							uni.removeStorageSync('isFill');
							this.showModal=true;
							this.showLoginModal();
							
						}
					})
					
				}else{
					this.showModal=true;
					this.showLoginModal();
					
				}
				
			},
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
				
			},
			showLoginModal(){
				
					
					this.$showModal({
						title: "温馨提示",
						content: '登录过后才可体验求职小程序',
						showCancel: true,
						cancelText: "取消",
						cancelColor: "#000000",
						confirmText: "登录",
						confirmColor: "#3CC51F",
						success: function(res) {
							if (res.confirm) {
								uni.navigateTo({
									url: '../login/login'
								})
							} else {
								// wx.showToast({
								//     title: '点击了取消',
								//     icon: 'none',
								//     duration: 2000
								// })
							}
						}
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
</style>
