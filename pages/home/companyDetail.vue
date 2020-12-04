<template>
	<view>
		<view class="main_cs">
			<view class="topView">
				<view class="topView_title">
					{{dataInf.companyName}}
				</view>
				<view class="topView_item">
					<image class="topView_item_img" src="../../static/cv/b.png" mode=""></image>
					<view class="topView_item_text">{{dataInf.industryCategory}}</view>
				</view>
				<view class="topView_item">
					<image class="topView_item_img" src="../../static/cv/b.png" mode=""></image>
					<view class="topView_item_text">{{dataInf.companyNature}}</view>
				</view>
				<view class="topView_item">
					<image class="topView_item_img" src="../../static/cv/b.png" mode=""></image>
					<view class="topView_item_text">{{dataInf.scope}}</view>
				</view>
			</view>

			<view class="title_flexView">
				<image class="title_flexView_img" src="../../static/home/star.png" mode=""></image>
				<view class="title_flexView_text">公司详情</view>
				<view class="title_flexView_line"></view>
			</view>

			<view class="descriptionView">
				{{dataInf.intro}}
			</view>
		</view>

		<view class="lineView">
			- 他们还在招 -
		</view>

		<view class="tab_list">
			<view class="cell" v-for="(item,index) in dataInf.positionArr" :key="index" @click="itemClick(item)">
				<view class="cell_titleview">
					<view class="cell_titleview_title">
						{{item.name}}
					</view>
					<view class="cell_titleview_salary">
						{{item.salaryRequirement}}
					</view>
				</view>
				<view class="cell_msg">
					{{item.location+'&nbsp|&nbsp'+item.expRequirement+'&nbsp|&nbsp'+item.educationRequirement}}
				</view>
				<!-- <view class="cell_bottomview">
					<view class="cell_bottomview_company">
						{{item.company}}
					</view>
					<view class="cell_bottomview_time">
						{{item.releaseTime}}
					</view>
				</view> -->
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
				positionId: '',
				companyId: '',


				dataInf: {}
			};
		},

		onLoad(e) {
			this.positionId = e.positionId
			this.companyId = e.companyId
			this.getData()
		},

		methods: {

			getData() {
				var loginkey = uni.getStorageSync('loginKey');
				if (loginkey) {
					this.$api.post('qzPosition!ajaxGetEnterpriseDetailInfo.action', {
						loginKey: loginkey,
						positionId: this.positionId,
						companyId: this.companyId

					}).then(res => {
						if (res.res.status == 0) {
							this.dataInf = res.inf
						} else {

						}
					})

				} else {
					this.showModal = true;
					this.showLoginModal();

				}
			},

			itemClick(item) {
				uni.navigateTo({
					url: './cvDetail?id=' + item.id
				})
			}
		}

	}
</script>

<style lang="scss">
	.main_cs {}

	.topView {
		padding: 10px;

		.topView_title {
			font-size: 14px;
			height: 30px;
			line-height: 30px;
		}

		.topView_item {
			display: flex;
			align-items: center;
			height: 25px;

			.topView_item_img {
				width: 10px;
				height: 10px;
				background-color: #007AFF;
			}

			.topView_item_text {
				height: 20px;
				line-height: 20px;
				font-size: 12px;
				margin-left: 10px;
			}
		}
	}

	.title_flexView {
		display: flex;
		height: 20px;
		align-items: center;
		margin-top: 10px;

		.title_flexView_img {
			height: 15px;
			width: 15px;
			margin: 10px;
		}

		.title_flexView_text {
			font-size: 14px;
			height: 20px;
			line-height: 20px;

		}

		.title_flexView_line {
			margin: 0px 10px;
			height: 1px;
			flex: 1;
			background-color: #e6e6e6;
		}
	}

	.descriptionView {
		font-size: 12px;
		padding: 10px;
		line-height: 20px;
		color: #666666;
		white-space: pre-wrap;
	}


	.lineView {
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
