<template>
	<view>
		<view class="topView">
			<image class="user_img" :src="userInfo.imgUrl?baseUrl+userInfo.imgUrl:'../../static/login/loginTX.png' " mode="" @click="setting()"></image>
			<view class="user_name">
				{{userName}}
			</view>
		</view>
		<view class="item" @click="interview()" >
			<image class="item_img" src="../../static/mine/mxyq.png" mode=""></image>
			<view  class="item_text">面试邀请</view>
			<image class="item_btn" src="../../static/mine/sjx.png" mode=""></image>
		</view>
		<view class="item" @click="jobprogress()" >
			<image class="item_img" src="../../static/mine/my_qz.png" mode=""></image>
			<view  class="item_text">求职进展</view>
			<image class="item_btn" src="../../static/mine/sjx.png" mode=""></image>
		</view>
		<view class="item" @click="shielding()" >
			<image class="item_img" src="../../static/mine/my_qz.png" mode=""></image>
			<view  class="item_text">屏蔽企业</view>
			<image class="item_btn" src="../../static/mine/sjx.png" mode=""></image>
		</view>
		<view class="item" @click="">
			<image class="item_img" src="../../static/mine/my_xx.png" mode=""></image>
			<view  class="item_text">学习进修</view>
			<image class="item_btn" src="../../static/mine/sjx.png" mode=""></image>
		</view>
		<view class="item" @click="mycerClick()">
			<image class="item_img" src="../../static/mine/my_zs.png" mode=""></image>
			<view  class="item_text">我的证书</view>
			<image class="item_btn" src="../../static/mine/sjx.png" mode=""></image>
		</view>
		<!-- <view class="item">
			<image class="item_img" src="../../static/mine/my_jl.png" mode=""></image>
			<view  class="item_text">我的简历</view>
			<image class="item_btn" src="../../static/mine/sjx.png" mode=""></image>
		</view> -->
		<view class="item">
			<image class="item_img" src="../../static/mine/my_xz.png" mode=""></image>
			<view  class="item_text">下载简历</view>
			<image class="item_btn" src="../../static/mine/sjx.png" mode=""></image>
		</view>
		

	</view>
</template>

<script>
	export default {
		data() {
			return {
				loginKey:'',
				baseUrl:'',
				userInfo: Object,
				userName:''
			}
		},
		onLoad() {
			this.baseUrl = getApp().globalData.baseUrl
		},
		
		onShow() {
			this.getuserInfo()
		},
		methods: {
			getuserInfo() {
				// 
				var loginkey = uni.getStorageSync('loginKey');
				if (loginkey){
					this.loginKey = loginkey;
					this.$api.post('user!ajaxGetUserInfo.action', {
						loginKey: loginkey
					}).then(res => {
						if (res.res.status == 0) {
							// this.sfz = res.inf.subCardNo
							this.userInfo = res.inf
							this.userName = this.userInfo.nickName
							console.log("have loginkey");
						} else {
							uni.removeStorageSync('loginKey');
							uni.removeStorageSync('userId');
							uni.removeStorageSync('isFill');
						}
					})
					
				}else{
					this.userInfo = null;
					this.userName = ''
					this.loginKey = ''
					uni.removeStorageSync('loginKey');
					uni.removeStorageSync('userId');
					uni.removeStorageSync('isFill');
				}
				
			},
			shielding(){
				uni.navigateTo({
					url:'./shieldingCompanies'
				})
			},
			
			interview(){
				uni.navigateTo({
					url:'./interview'
				})
			},
			setting(){
				
				if(this.loginKey.length>0){
					uni.navigateTo({
						url:'./mineSetting'
					})
				}else{
					uni.navigateTo({
						url:'../login/login'
					})
				}
				
			},
				
			jobprogress(){
				uni.navigateTo({
					url:'./jobProgress'
				})
			},
			
				
			mycerClick(){
				uni.navigateTo({
					url:'./myCertificate'
				})
			},
			
				
			jobManageClick(){
				uni.navigateTo({
					url:'./jobManage'
				})
			}
			
		}
	}
</script>

<style lang="scss">
	.topView {
		width: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
		flex-direction: column;
		height: 150px;

		.user_img {
			width: 80px;
			height: 80px;
			border-radius: 40px;
			background-color: #e8654b;
		}

		.user_name {
			font-size: 14px;
			line-height: 20px;
			height: 25px;
			line-height: 25px;
		}
	}
	
	.item{
		margin: 0px 15px 10px 15px;
		background-color:  #f5f7f8;
		// height: 20px;
		display: flex;
		align-items: center;
		padding: 10px;
		border-radius: 5px;
		.item_img{
			width: 15px;
			height: 15px;
			// background-color: #FFA200;
		}
		.item_text{
			margin-left: 10px;
			font-size: 14px;
			flex: 1;
		}
		
		.item_btn{
			width: 7px;
			height: 10px;
		}
	}
</style>
