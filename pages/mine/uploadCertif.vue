<template>
	<view>
		<view class="itemview" style="margin-top: 10px;">
			<view class="itemview_title">证书名称</view>
			<input class="itemview_text" type="text" value="" />
		</view>
		<view class="itemview">
			<view class="itemview_title">证书编号</view>
			<input class="itemview_text" type="text" value="" />
		</view>
		<view class="itemview">
			<view class="itemview_title">颁发机构</view>
			<input class="itemview_text" type="text" value="" />
		</view>
		<view class="itemview">
			<view class="itemview_title">颁发日期</view>
			<input class="itemview_text" type="text" value="" />
		</view>
		<view class="itemview">
			<view class="itemview_title">证书照片</view>
			<image class="itemview_picture" :src="picStr.length>0?picStr:'../../static/mine/addpicture.png' " mode="widthFix" @click="pictureClick()"></image>
		</view>

		<view class="commit_btn">
			上传
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				picStr:''
			};
		},

		methods: {

			pictureClick() {
				console.log("用户点击了从图库上传");
				uni.chooseImage({
					count: 1,
					sizeType: ["compressed"],
					success(response) {



						// 选择图片后, 返回的数据
						var fileUrl = response.tempFilePaths[0]
						_this.picStr = fileUrl
						console.log("用户点击了从图库上传222 =" + fileUrl);
					}
				})
			}
		}
	}
</script>

<style lang="scss">
	.itemview {
		padding: 10px;

		.itemview_title {
			height: 20px;
			line-height: 20px;
			font-size: 14px;
			color: #333333;
		}

		.itemview_text {
			margin-top: 10px;
			height: 20px;
			line-height: 20px;
			border-radius: 5px;
			background-color: #f5f7f8;
			font-size: 14px;
			color: #666666;
			padding: 5px;
		}

		.itemview_picture {
			margin: 10px 0px;
			// background-size:contain|cover;
			width: 100%;
			height: auto;
			border-radius: 5px;
			background-color: #f5f7f8;
		}
	}


	.commit_btn {
		margin-top: 20px;
		width: 80%;
		margin-left: 10%;
		height: 40px;
		border-radius: 5px;
		background-color: #e8654b;
		text-align: center;
		line-height: 40px;
		color: #FFFFFF;
		font-size: 14px;
	}
</style>
