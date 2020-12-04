<template>
	<view>
		<view class="list">
			<view class="cell" v-for="(item,index) in dataArr" :key="index">
				<image v-if="item.isSelected===1" class="cell_img" src="../../static/cv/tb_selected.png" mode="" @click="itemSelect(item)"></image>
				<image v-if="item.isSelected===0" class="cell_img" src="../../static/cv/tb_normal.png" mode="" @click="itemSelect(item)"></image>
				<view class="cell_contentView" @click="itemSelect(item)">
					<view class="cell_contentView_title">
						{{item.name}}
					</view>
					<view class="cell_contentView_jg">
						{{item.awardDate+zwf+item.organization}}
					</view>
				</view>
				<image class="cell_endBtn" src="../../static/cv/sz.png" mode="" @click="settingCer(item)"></image>
			</view>
		</view>


		<!-- <view class="bottom_view">
			<view class="bottom_btn" @click="uploadClick()">
				上传证书
			</view>
		</view>
 -->

		<view class="bottom_view">
			<view class="bottom_view_cz" @click="clickNew()">新建</view>
			<view class="bottom_view_qd" @click="clickOK()">确定</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				id: '',
				zwf:'\u3000',
				dataArr: []
			};
		},

		onLoad(e) {
			this.id = e.id

			if (this.id) {
				this.getData()
			}

		},
		
		onShow() {
			if (this.id) {
				this.getData()
			}
		},


		methods: {
			getData() {
				var loginkey = uni.getStorageSync('loginKey');
				this.$api.post('resume!ajaxGetResumeCertificationList.action', {
					loginKey: loginkey,
					resumeId: this.id
				}).then(res => {
					if (res.res.status == 0) {
						this.dataArr = res.inf.arr
					} else {
						uni.showToast({
							title: res.res.error
						})
					}

				})

			},
			clickNew(){
				uni.navigateTo({
					url:'../mine/uploadCertif'
				})
			},
			
			clickOK() {
				//
				
				var selectArr = []
				for(var i =0;i<this.dataArr.length;i++ ){
					let item = this.dataArr[i];
					if(item.isSelected ===1){
						selectArr.push(item.id)
					}
				}
				var loginkey = uni.getStorageSync('loginKey');
				this.$api.post('resume!ajaxAddResumeCertificate.action', {
					loginKey: loginkey,
					resumeId: this.id,
					certificateIdArr:JSON.stringify(selectArr)
				}).then(res => {
					if (res.res.status == 0) {
						uni.navigateBack({
							
						})
					} else {
						uni.showToast({
							title: res.res.error
						})
					}
				
				})
			},

			
			
				
			settingCer(item){
				uni.navigateTo({
					url:'../mine/uploadCertif?id='+item.id
				})
			},

			itemSelect(item) {
				if (item.isSelected === 0) {
					item.isSelected = 1

				} else {
					item.isSelected = 0
				}
			}

		}
	}
</script>

<style lang="scss">
	.list {
		padding: 10px;

		.cell {
			margin: 10px 0px;
			padding: 10px;
			display: flex;
			align-items: center;
			background-color: #f5f7f8;
			border-radius: 5px;

			.cell_img {
				width: 15px;
				height: 15px;
				// border-radius: 15px;
				// background-color: #e8654b;
			}

			.cell_contentView {
				display: flex;
				flex-direction: column;
				justify-content: center;
				margin-left: 10px;
				flex: 1;

				.cell_contentView_title {
					font-size: 14px;
					height: 20px;
					line-height: 20px;

				}

				.cell_contentView_jg {
					font-size: 12px;
					color: #666666;
					height: 20px;
					line-height: 20px;
				}
			}


			.cell_endBtn {
				width: 20px;
				height: 20px;
			}

		}
	}


	// .bottom_view {
	// 	width: 100%;
	// 	height: 60px;
	// 	display: flex;
	// 	justify-content: center;
	// 	align-items: center;
	// 	position: fixed;
	// 	bottom: 0;

	// 	.bottom_btn {

	// 		margin: 10px;
	// 		 width: 80%;
	// 		height: 40px;
	// 		background-color: #e8654b;
	// 		font-size: 14px;
	// 		color: #FFFFFF;
	// 		line-height: 40px;
	// 		text-align: center;
	// 		border-radius: 5px;
	// 	}

	// 	.bottom_btn {
	// 		border: 2px solid #e8654b;
	// 		background-color: #fbc9bc;
	// 		color: #e8654b;
	// 		font-size: 14px;
	// 		border-radius: 5px;
	// 		width: 80px;
	// 		height: 40px;
	// 		margin: 10px;
	// 		text-align: center;
	// 		line-height: 30px;
	// 	}
	// }

	.bottom_view {
		margin-top: 30px;
		height: 50px;
		display: flex;
		justify-content: center;
		width: 100%;
		position: fixed;
		bottom: 0;

		.bottom_view_cz {
			border: 2px solid #e8654b;
			background-color: #fbc9bc;
			color: #e8654b;
			font-size: 14px;
			border-radius: 5px;
			width: 40%;
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
			margin: 10px 10px 10px 0px;
			background-color: #e8654b;
			color: #FFFFFF;
			font-size: 14px;
			text-align: center;
			line-height: 30px;

		}
	}
</style>
