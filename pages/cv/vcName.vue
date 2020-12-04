<template>
	<view>

		<view class="item_views">
			<view class="item_title">
				简历名称
			</view>
			<view class="item">
				<input class="item_content" type="text" value="" v-model="schoolName" />
			</view>
		</view>
		<view class="bottom_view">
			<view class="bottom_view_qd" @click="save()">保存</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			
			return {
				zwf: '\u3000',
				schoolName: '',
				professional:'',
				beginTime: '',
				endTime: '至今',
				typelist: ['1', '2', '3', '4'],
				xltype: '',
				haveXl:true,
				xlarr:['不限','初中及以下','中专/中技','高中','大专','本科','硕士','MBA/EMBA','博士'],
				resumeId:''
			};
		},


		onLoad(e) {
			this.resumeId = e.id
		},
		methods: {
			
			save(){
				var loginkey = uni.getStorageSync('loginKey');
				this.$api.post('resume!ajaxUpdateResumeTitle.action', {
					loginKey: loginkey,
					resumeId:this.resumeId,
					resumeTitle:this.schoolName
					
				}).then(res => {
				if (res.res.status == 0) {
					uni.showToast({
						title: '修改成功',
						complete() {
							uni.navigateBack({
								
							})
						}
					})
				} else {
					uni.showToast({
						title: res.res.error
					})
				}		
				
				})
			}
		}
	}
</script>

<style lang="scss">
	.item_views {
		margin-top: 10px;
		display: flex;
		align-items: center;
		padding: 10px;

		.item_title {
			margin-right: 10px;
			font-size: 14px;
			height: 20px;
			line-height: 20px;
		}

		.item {
			display: flex;
			margin: 0px 0px 0px 0px;
			border-radius: 5px;
			background-color: #f5f7f8;
			font-size: 14px;
			color: #666666;

			flex: 1;

			.item_content {
				flex: 1;
				padding: 5px;

			}

			.item_imgView {
				width: 30px;
				height: 30px;
				background-color: #e8654b;
				border-top-right-radius: 5px;
				border-bottom-right-radius: 5px; //右下角

				display: flex;
				align-items: center;
				justify-content: center;

				.item_img {
					width: 8px;
					height: 5px;
				}
			}

		}
	}

	.bottom_view {
		position: fixed;
		bottom: 0px;
		height: 50px;
		display: flex;
		justify-content: center;
		width: 100%;

		.bottom_view_cz {
			border: 2px solid #e8654b;
			background-color: #fbc9bc;
			color: #e8654b;
			font-size: 17px;
			border-radius: 5px;
			width: 80px;
			height: 30px;
			margin: 10px;
			text-align: center;
			line-height: 30px;
		}

		.bottom_view_qd {
			border: 2px solid #e8654b;
			border-radius: 5px;
			height: 30px;
			flex: 1;
			margin: 10px 10px 10px 10px;
			background-color: #e8654b;
			color: #FFFFFF;
			font-size: 17px;
			text-align: center;
			line-height: 30px;

		}
	}

	.desView {
		margin: 5px 10px;
		padding: 10px;
		width: calc(100vw - 40px);
		background-color: #f5f7f8;
		height: 60px;
		font-size: 14px;
	}

	.sw {
		transform: scale(0.7, 0.7);
	}

	.selectxl{
		display: flex;
		width: 100px;
		height: 20px;
		border-radius: 5px;
		border: 1px solid #e8654b;
		background-color:#e8654b; 
		.selectxl_item{
			margin: 0px;
			font-size: 12px;
			color: #FFFFFF;
			border-radius: 5px;
			text-align: center;
			line-height: 20px;
			flex: 1;
			background-color: #FFFFFF;
		}
		
		.isSelect{
			background-color: #e8654b;
		}
	}
</style>
