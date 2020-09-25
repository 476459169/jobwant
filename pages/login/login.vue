<template>
	<view>
		<view class="headerView">
			<image class="user_header" src="../../static/login/loginTX.png" mode=""></image>
		</view>
		<view class="user_header_text">登录或注册临研招聘</view>
		
		<button class="btn btn_top" open-type="getPhoneNumber" @getphonenumber="getPhoneNumber"> 微信登录</button>
		
		<button class="btn"  @click="phoneLogin"> 手机号注册/登录</button>
	</view>
</template>

<script>
	export default{
		data(){
			return{
				
			}
		},
		methods:{
				
			getPhoneNumber(e){
				
				console.log(e.detail.errMsg)
				    console.log(e.detail.iv)
				    console.log(e.detail.encryptedData)
			},
			phoneLogin(){
				console.log(1111) 
				uni.navigateTo({
					url:'./phoneLogin'
				})
			}
		}
	}
</script>

<style lang="scss">
	
	.headerView{
		display: flex;
		justify-content: center;
		width: 100%;
		.user_header{
			margin-top: 50px;
			width: 100px;
			height: 100px;
			border-radius: 50px;
			background-color: #e8654b;
		}
		
	}
	.user_header_text{
		text-align: center;
		font-size: 14px;
		width: 100%;
		height: 20px;
		margin-top: 10px;
		
	}
	
	
	.btn{
		margin-top: 10px;
		background-color: #FFFFFF;
		text-align: center;
		height: 35px;
		width: 80%;
		margin-left: 10%;
		border-radius: 5px;
		border: 1px solid #000000;
		font-size: 14px;
	}
	
	.btn_top{
		margin-top: 200px;
	}
	
</style>
