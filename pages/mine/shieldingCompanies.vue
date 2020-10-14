	
	
<template>
	
	<view>
		<view class="item_views">
			<view class="item" v-for="(item,index) in dataArr" :key="index">
				<view class="item_content">{{item.companyName}}</view>
				<view class="item_imgView">
					移除
				</view>
			</view>
		</view>
		
		<view class="bottom_view">
			<view class="bottom_view_qd"  @click="addcompany()">添加公司</view>
		</view>
	</view>
</template>

<script>
	export default{
		data(){
			return{
					
				dataArr:[{
					companyName:'临语堂',
				},{
					companyName:'临语堂',
				},{
					companyName:'临语堂',
				}
					
				]
			}
		},
		methods:{
			addcompany(){
				uni.navigateTo({
					url:'./addShieldingCompanies'
				})
			}
		}
		
	}
</script>

<style lang="scss">
	.item_views {
		margin-top: 10px;
		// display: flex;
		// align-items: center;
		padding: 10px;
	
		.item_title {
			margin-right: 10px;
			font-size: 14px;
			height: 20px;
			line-height: 20px;
		}
	
		.item {
			display: flex;
			margin: 0px 0px 10px 0px;
			border-radius: 5px;
			background-color: #f5f7f8;
			font-size: 14px;
			color: #666666;
	
			flex: 1;
	
			.item_content {
				flex: 1;
				// padding: 5px;
				margin: 5px;
				line-height: 30px;
	
			}
	
			.item_imgView {
				width: 50px;
				height: 40px;
				background-color: #e8654b;
				border-top-right-radius: 5px;
				border-bottom-right-radius: 5px; //右下角
	
				display: flex;
				align-items: center;
				justify-content: center;
				font-size: 12px;
				text-align: center;
				line-height: 40px;
				color: #FFFFFF;
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
		font-size: 14px;
		.bottom_view_cz {
			border: 2px solid #e8654b;
			background-color: #fbc9bc;
			color: #e8654b;
			// font-size: 17px;
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
			// font-size: 17px;
			text-align: center;
			line-height: 30px;
	
		}
	}
</style>
