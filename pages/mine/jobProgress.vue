<template>
	<view>

		<view class="top_fixed_view">
			<view class="top_view">
				<view class="top_view_item" :class="[topSelect == item?'top_view_select_color':'']" v-for="(item,index) in topArr"
				 :key="index" @click="topItemClick(item)">
					{{item}}
				</view>
			</view>
		</view>

		<view class="list">
			<view class="" v-for="(item,index) in data" :key="index">
				<view class="cell">

					<view class="cell_endView">
						<view class="cell_endView_img"></view>
					</view>
					<view class="cell_content">
						<view class="cell_content_title">{{item.company}}</view>
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
				topArr: ['全部','已沟通', '已约面', '面试待接收', '面试已接收', '已面试', '未面试', '面试已拒绝', '不合适'],
				topSelect: '全部',
				data: [{
						company: '临语堂（天津）健康管理有限公司',
						progress: '感谢您的投递，我会尽快查看您的简历感谢您的投递，我会尽快查看您的简历感谢您的投递，我会尽快查看您的简历',
						status: '已投递'
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
		padding: 20px 0px;

		.top_view {
			position: relative;
			display: flex;
			flex-wrap: wrap;

			.top_view_item {
				background-color: #fbc9bc;
				color: #8f9291;
				font-size: 14px;
				border-radius: 5px;
				width: 30%;
				height: 25px;
				margin-top: 10px;
				margin-left: 10px;
				padding: 3px 0px;
				text-align: center;
				line-height: 25px;
			}

			.top_view_select_color {
				color: #e8654b;
			}
		}
	}


	.list {
		margin-top: 160px;

		.cell {
			display: flex;
			align-items: center;
			padding: 10px;

			.cell_content {
				display: flex;
				flex-direction: column;
				justify-content: center;
				flex: 1;

				.cell_content_title {
					font-size: 14px;
					line-height: 25px;
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
					font-size: 12px;
					line-height: 15px;
					height: 15px;
					width: 60px;
					color: #666666;
					text-align: center;
					border: 1px solid #666666;
					border-radius:10px;
					margin: 5px 0px;
					padding: 2.5px;
				}
			}

			.cell_endView {
				display: flex;
				flex-direction: column;
				justify-content: center;
				align-items: center;

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
			}
		}
	}

	.line {
		margin: 0px 10px;
		height: 1px;
		background-color: #f5f7f8;

	}
</style>
