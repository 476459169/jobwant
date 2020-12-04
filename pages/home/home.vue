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

		<view class="tab_list">
			<view class="cell" v-for="(item,index) in data" :key="index">

				<view class="cell_mainV">
					<!-- <view class="cell_btn" :class="[item.select==true?'cell_btn_select':'']" @click="itemSelect(item)">

					</view> -->

					<image v-if="item.isSelected===true" class="cell_img" src="../../static/cv/tb_selected.png" mode="" @click="itemSelect(item)"></image>
					<image v-if="item.isSelected===false" class="cell_img" src="../../static/cv/tb_normal.png" mode="" @click="itemSelect(item)"></image>

					<view class="cell_content" @click="itemClick(item)">
						<view class="cell_titleview" :class="[item.isDelivery===true?'itemisDelivery':'']">
							<view class="cell_titleview_title">
								{{item.name}}
							</view>
							<view class="cell_titleview_salary" :class="[item.isDelivery===true?'cell_isDelivery':'']">
								{{item.salaryRequirement}}
							</view>
						</view>
						
						<view class="cell_msgViews" :class="[item.isDelivery===true?'itemisDelivery':'']">
							<view class="cell_msg" :class="[item.isDelivery===true?'cell_isDelivery':'']" >
								{{item.location+'&nbsp|&nbsp'+item.expRequirement+'&nbsp|&nbsp'+item.educationRequirement}}
							</view>
							<view class="cell_status">
								{{item.isDelivery===true?'已投递':''}}
							</view>
						</view>
						
						<view class="cell_bottomview" :class="[item.isDelivery===true?'itemisDelivery':'']">
							<view class="cell_bottomview_company" :class="[item.isDelivery===true?'cell_isDelivery':'']">
								{{item.companyName}}
							</view>
							<view class="cell_bottomview_time" >
								{{item.upDate}}
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
	var _this;
	var timer = null;
	export default {
		data() {
			return {
				loginkey: '',
				showModal: true,
				baseUrl: "https://uat.crlink.com/crlink/",
				data: [],
				page: 1,
				selectArr:[],
				shieldInf: {
					// selEducationRequirement:'学历不限',
					// selExperienceRequirement:'经验不限',
					// selCompanyNatureRequirement:'全部',
					// selCompanyScope:'',
					// selWorkLocation:'',	

					selEducationRequirement: '',
					selExperienceRequirement: '',
					selCompanyNatureRequirement: '',
					selCompanyScope: '',
					selWorkLocation: '',
				}

			};
		},

		onLoad() {
			_this = this
			this.baseUrl = getApp().globalData.baseUrl
			this.getuserInfo();
			this.getData()

		},

		onShow() {

			this.getuserInfo();

			if (this.shieldInf.reload === '1') {
				_this.page = 1
				this.getData()
			}
			console.log('selCompanyScope=' + this.shieldInf.selCompanyScope);

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

			getData() {
				var loginkey = uni.getStorageSync('loginKey');
				if (loginkey) {
					this.$api.post('qzPosition!ajaxGetPositionList.action', {
						loginKey:loginkey,
						selName: '',
						selEducationRequirement: this.shieldInf.selEducationRequirement,
						selExperienceRequirement: this.shieldInf.selExperienceRequirement,
						selCompanyNatureRequirement: this.shieldInf.selCompanyNatureRequirement,
						selCompanyScope: this.shieldInf.selCompanyScope,
						selWorkLocation: this.shieldInf.selWorkLocation,
						firstIndex: _this.page
					}).then(res => {
						if (res.res.status === 0) {
							this.shieldInf.reload = '0'
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
							uni.hideNavigationBarLoading();
							uni.stopPullDownRefresh(); //数据加载完成,刷新结束
						} else {

						}
					})

				} else {
					this.showModal = true;
					this.showLoginModal();

				}

			},
			getuserInfo() {
				// 
				var loginkey = uni.getStorageSync('loginKey');
				if (loginkey) {
					this.$api.post('user!ajaxGetUserInfo.action', {
						loginKey: loginkey
					}).then(res => {
						if (res.res.status == 0) {
							// this.sfz = res.inf.subCardNo
							console.log("have loginkey");
							this.showModal = false;

						} else {
							uni.removeStorageSync('loginKey');
							uni.removeStorageSync('userId');
							uni.removeStorageSync('isFill');
							this.showModal = true;
							this.showLoginModal();

						}
					})

				} else {
					this.showModal = true;
					this.showLoginModal();

				}

			},
			screenClick() {

				uni.navigateTo({
					url: './screen?shieldInf=' + encodeURIComponent(JSON.stringify(this.shieldInf))
				})
			},

			itemClick(item) {
				uni.navigateTo({
					url: './cvDetail?id=' + item.id
				})
			},
			itemSelect(item) {
				console.log('itemselect');
				
				if(item.isDelivery===false){
					item.isSelected = !item.isSelected
					if (this.selectArr.indexOf(item) == -1) {
						this.selectArr.push(item)
					} else {
						var index1 = this.selectArr.indexOf(item);
						this.selectArr.splice(index1, 1)
					}
				}
				
				
			},
			datacommit() {
				
					var loginkey = uni.getStorageSync('loginKey');
					if (loginkey) {
						this.$api.post('qzPosition!ajaxBatchDelivery.action', {
							loginKey: loginkey,
							deliveryPositionArr:JSON.stringify(this.selectArr)
						}).then(res => {
							if (res.res.status == 0) {
								uni.showToast({
									title:'投递成功'
								})
								this.page=1
								this.getData()
							} else {
								uni.showToast({
									title:res.res.error
								})
							}
						})
					}
				
				
			},
			showLoginModal() {


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

			.cell_img {
				margin: 37px 10px 10px 10px;
				width: 15px;
				height: 15px;
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
				
				.cell_isDelivery{
					color: #CCCCCC;
				}

			}

			.cell_msgViews{
				display: flex;
				align-items: center;
				.cell_msg {
					font-size: 12px;
					color: #333333;
					flex: 1;
				}
				
				.cell_status{
					font-size: 12px;
					color: #CCCCCC;
				}
				
				.cell_isDelivery{
					color: #CCCCCC;
				}
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
				
				.cell_isDelivery{
					color: #CCCCCC;
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
	
	
	.itemisDelivery{
		color: #cccccc;
	}
	
</style>
