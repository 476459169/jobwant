<template>
	<view>
		<view class="list">
			<view class="cell" v-for="(item,index) in dataArr" :key="index">
				<image class="cell_img" src="../../static/mine/mycer.png" mode=""></image>
				<view class="cell_contentView">
					<view class="cell_contentView_title">
						{{item.name}}
					</view>
					<view class="cell_contentView_jg">
						{{item.awardDate+'-'+item.organization}}
					</view>
				</view>
				<!-- <image class="cell_endBtn" src="../../static/cv/sz.png" mode="" @click="itemClick(item)"></image> -->
				<view class="click_btns">
					<view class="change_btn"  @click="itemClick(item)">
						修改
					</view>
					<view class="delete_btn"  @click="deleteItem(item)">
						删除
					</view>
				</view>
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
				id:'',
				dataArr:[]
			};
		},
		
		onLoad(e) {
			// this.id = e.id
			
		},
		
		onShow() {
			this.getData()
		},
		methods:{
				
			uploadClick(){
				uni.navigateTo({
					url:'./uploadCertif'
				})
			},
			
				
			getData(){
				
				
				var loginkey = uni.getStorageSync('loginKey');
				this.$api.post('resume!ajaxGetCertificationList.action', {
					loginKey: loginkey,
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
			
			itemClick(item){
				uni.navigateTo({
					url:'./uploadCertif?id='+item.id
				})
			},
			
			deleteItem(item){
				var loginkey = uni.getStorageSync('loginKey');
				this.$api.post('resume!deleteCertification.action', {
					loginKey: loginkey,
					certificationId:item.id
				}).then(res => {
					if (res.res.status == 0) {
						this.getData()
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
	
	.click_btns{
		display: flex;
		align-items: center;
		.change_btn{
			font-size: 12px;
			color: #e8654b;
			line-height: 30px;
			height: 30px;
			padding: 10px;
		}
		
		.delete_btn{
			font-size: 12px;
			color: #e8654b;
			line-height: 30px;
			height: 30px;
			padding: 10px;
		}
	}
</style>
