<template>
	<view>
		
		<view class="item_views">
			<view class="item_title">
					期望职位
			</view>
			<view class="item" @tap="handleTap('picker333')">
				<view class="item_content">{{type}}</view>
				<view class="item_imgView">
					<image class="item_img" src="../../static/login/star3.png" mode=""></image>
				</view>
			</view>
		</view>
		
		<lb-picker ref="picker333"
		  v-model="type"
		  mode="selector"
		  :list="typelist">
		  </lb-picker>
		
	
		<view class="item_views">
			<view class="item_title">
					工作城市
			</view>
			<view class="item" @tap="handleTap('picker334')">
				<view class="item_content">{{type}}</view>
				<view class="item_imgView">
					<image class="item_img" src="../../static/login/star3.png" mode=""></image>
				</view>
			</view>
		</view>
		<lb-picker ref="picker334"
		  v-model="type"
		  mode="selector"
		  :list="typelist">
		  </lb-picker>
		
		<view class="item_views">
			<view class="item_title">
					薪资范围
			</view>
			<view class="item" @tap="handleTap('picker335')">
				<view class="item_content">{{type}}</view>
				<view class="item_imgView">
					<image class="item_img" src="../../static/login/star3.png" mode=""></image>
				</view>
			</view>
		</view>
		<lb-picker ref="picker335"
		  v-model="type"
		  mode="selector"
		  :list="typelist">
		  </lb-picker>
		
		<view class="item_views">
			<view class="item_title">
					工作性质
			</view>
			<view class="item" @tap="handleTap('picker336')">
				<view class="item_content">{{type}}</view>
				<view class="item_imgView">
					<image class="item_img" src="../../static/login/star3.png" mode=""></image>
				</view>
			</view>
		</view>
		<lb-picker ref="picker336"
		  v-model="type"
		  mode="selector"
		  :list="typelist">
		  </lb-picker>
		  
		  
		  <view class="bottom_view">
		  	<view class="bottom_view_cz">删除</view>
		  	<view class="bottom_view_qd">保存</view>
		  </view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				typelist:['1','2','3','4'],
				type:''
			};
		},
		methods:{
				
			handleTap(picker) {
				this.$refs[picker].show()
			}
		}
	}
</script>

<style lang="scss">
	
	.item_views{
		margin-top: 30px;
		display:  flex;
		align-items: center;
		padding: 10px;
		.item_title{
			margin-right:10px ;
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
			margin: 10px 10px 10px 0px;
			background-color: #e8654b;
			color: #FFFFFF;
			font-size: 17px;
			text-align: center;
			line-height: 30px;
	
		}
	}
	
</style>
