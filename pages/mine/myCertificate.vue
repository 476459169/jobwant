<template>
	<view>
		<view class="list">
			<view class="cell" v-for="(item,index) in dataArr" :key="index">
				<image class="cell_img" src="../../static/mine/mycer.png" mode=""></image>
				<view class="cell_contentView">
					<view class="cell_contentView_title">
						{{item.certifName}}
					</view>
					<view class="cell_contentView_jg">
						{{item.certifJG}}
					</view>
				</view>
				<image class="cell_endBtn" src="../../static/cv/sz.png" mode=""></image>
			</view>
		</view>
		
		
		<view class="bottom_view" >
			<view class="bottom_btn" @click="uploadClick()">
				上传证书
			</view>
		</view>
		
	</view>
</template>

<script>
	export default{
		data(){
			return{
				dataArr:[
					{
						certifName:'CRA学院证书',
						certifJG:'临语堂（天津）健康管理有限公司',
						
					},
					{
						certifName:'CRA学院证书',
						certifJG:'临语堂（天津）健康管理有限公司'
					}
				]
			};
		},
		
		
		methods:{
				
			uploadClick(){
				uni.navigateTo({
					url:'./uploadCertif'
				})
			}
		}
	}
</script>

<style lang="scss">
	
	.list{
		padding: 10px;
		.cell{
			margin: 10px 0px;
			padding: 10px;
			display: flex;
			align-items: center;
			background-color: #f5f7f8;
			border-radius: 5px;
			.cell_img{
				width: 30px;
				height: 30px;
				border-radius: 15px;
				background-color: #e8654b;
			}
			
			.cell_contentView{
				display: flex;
				flex-direction: column;
				justify-content: center;
				margin-left: 10px;
				flex: 1;
				.cell_contentView_title{
					font-size: 14px;
					height: 20px;
					line-height: 20px;
					
				}
				.cell_contentView_jg{
					font-size: 12px;
					color: #666666;
					height: 20px;
					line-height: 20px;
				}
			}
			
				
			.cell_endBtn{
				width: 20px;
				height: 20px;
			}
			
		}
	}
	
		
	.bottom_view{
		width: 100%;
		height: 60px;
		display: flex;
		justify-content: center;
		align-items: center;
		position: fixed;
		bottom: 0;
		.bottom_btn{
		
			width: 80%;
			height: 40px;
			background-color: #e8654b;
			font-size: 14px;
			color: #FFFFFF;
			line-height: 40px;
			text-align: center;
			border-radius: 5px;
		}
	}
</style>
