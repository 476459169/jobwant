<template>
	<view>

		<view class="item_views">
			<view class="item_title">
				学校名称
			</view>
			<view class="item">
				<input class="item_content" type="text" value="" v-model="schoolName" />
			</view>
		</view>




		<view class="item_views">
			<view class="item_title">
				学{{zwf+zwf}}历
			</view>
			<view class="item" @tap="handleTap('picker333')">
				<view class="item_content">{{type}}</view>
				<view class="item_imgView">
					<image class="item_img" src="../../static/login/star3.png" mode=""></image>
				</view>
			</view>
		</view>
		<lb-picker ref="picker333" v-model="type" mode="selector" :list="typelist">
		</lb-picker>

		<view class="item_views">
			<view class="item_title">
				学位证书
			</view>


			<view class="selectxl" @click="selectxlClick()">
				<view class="selectxl_item"  :class="[haveXl==true?'isSelect':'']">
					有
				</view>
				<view class="selectxl_item" :class="[haveXl==true?'':'isSelect']">
					无
				</view>
			</view>

		</view>

		<view class="item_views">
			<view class="item_title">
				所学专业
			</view>
			<view class="item">
				<input class="item_content" type="text" value="" v-model="schoolName" />
			</view>
		</view>

		<view class="item_views">
			<view class="item_title">
				在校时间
			</view>
			<view class="item" @tap="handleTap('pickerdate1')">
				<view class="item_content">{{beginTime}}</view>
				<view class="item_imgView">
					<image class="item_img" src="../../static/login/star3.png" mode=""></image>
				</view>
			</view>
			<view class="item_title" style="margin-left: 5px;">
				-
			</view>
			<view class="item" @tap="handleTap('pickerdate2')">
				<view class="item_content">{{endTime}}</view>
				<view class="item_imgView">
					<image class="item_img" src="../../static/login/star3.png" mode=""></image>
				</view>
			</view>
		</view>
		<e-picker-plus ref="pickerdate1" @confirm="confirm1" mode="YM" :endRule=currentDate> </e-picker-plus>
		<e-picker-plus ref="pickerdate2" @confirm="confirm2" mode="YM" :endRule=currentDate> </e-picker-plus>

		<view class="bottom_view">
			<view class="bottom_view_qd">保存</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			const currentD = this.getDate({
				format: true
			})
			return {
				zwf: '\u3000',
				currentDate: currentD,
				schoolName: '',
				jobName: '',
				beginTime: '',
				endTime: '至今',
				industry: '',
				salary: '',
				workDes: '',
				typelist: ['1', '2', '3', '4'],
				type: '',
				switchValue: false,
				haveXl:true
			};
		},


		onLoad() {},
		methods: {
			getDate(type) {
				const date = new Date();

				let year = date.getFullYear();
				let month = date.getMonth() + 1;
				let day = date.getDate();

				if (type === 'start') {
					year = year - 60;
				} else if (type === 'end') {
					year = year + 2;
				}
				month = month > 9 ? month : '0' + month;;
				day = day > 9 ? day : '0' + day;
				let ztime = `${year}-${month}`
				return ztime;
			},

			handleTap(picker) {
				this.$refs[picker].show()
			},
			switchClick() {
				this.switchValue = !this.switchValue;
				console.log('this.switch = ' + this.switchValue);
			},
			confirm1(e) {
				this.beginTime = e.result
				console.log(e.result)
			},
			confirm2(e) {
				if (e.result === this.currentDate) {
					this.endTime = '至今'
				} else {
					this.endTime = e.result
				}
			},
			selectxlClick(){
				this.haveXl = !this.haveXl
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
