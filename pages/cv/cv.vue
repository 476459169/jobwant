<template>
	<view>
		
		
		 <d-alert v-if="showModal"></d-alert>
		 
		 <view class="" v-if="loginKey.length>0">
		 	<view class="topView">
		 	
		 		<view class="topView_nameView">
		 			<view class="topView_nameView_name">
		 				{{dataInfo.realName.length>0?dataInfo.realName:'姓名'}}
		 			</view>
		 			<view class="topView_nameView_xb">
		 				{{dataInfo.sexComment.length>0?'/'+dataInfo.sexComment:''}}
		 			</view>
		 			<image class="rightBtn" src="../../static/cv/bj.png" mode="" @click="userMsg()"> </image>
		 		</view>     
		 	
		 		<view v-if="dataInfo.workYears.length>0">
		 			<view class="topView_msg">
		 				{{dataInfo.userAge+'&nbsp|&nbsp'+dataInfo.workYears+'&nbsp|&nbsp'+dataInfo.livingLocation}}
		 			</view>
		 	
		 			<view class="view_item">
		 				<image class="view_item_img" src="../../static/cv/phone.png" mode=""></image>
		 				<view class="view_item_text"> {{dataInfo.contactPhoneNum}}</view>
		 			</view>
		 	
		 			<view class="view_item">
		 				<image class="view_item_img" src="../../static/cv/yx.png" mode=""></image>
		 				<view class="view_item_text">{{dataInfo.email}}</view>
		 			</view>
		 		</view>
		 	</view>
		 	<view class="line"></view>
		 	
		 	<view class="mycv">
				
				<view class="plateItemview_titleView">
					<view class="plateItemview_titleView_title">
						我的简历
					</view>
					<image class="rightBtn" src="../../static/cv/tj.png" mode="" @click="addcv()"></image>
				</view>
		 		
		 			
		 		<view class="cv_item" v-for="(item,index) in dataInfo.resumeArr" :key = "index">
		 			<view class="cv_item_point"></view>
		 			<view class="cv_item_text">
		 				{{item.title}}
		 			</view>
		 			
		 			<view class="cv_item_settingviews">
		 				<view class="cv_item_settingviews_item" @click="showcv(item.id)">预览</view>
		 				<view class="cv_item_settingviews_item" >下载</view>
		 				<view class="cv_item_settingviews_item" @click="seettingCv(item.id)">修改</view>
		 				<view class="cv_item_settingviews_item" @click="removeCv(item.id)">删除</view>
		 			</view>
		 		</view>
		 	</view>
			<view class="line"></view>
		 	
		 	<view class="mycv">
		 		<view class="cv_title">导入简历</view>	
		 		<view class="cv_item">
		 			<!-- <view class="cv_item_point">
		 				
		 			</view>
		 			<view class="cv_item_text">
		 				CRA简历
		 			</view>
		 			
		 			<view class="cv_item_settingviews">
		 				<view class="cv_item_settingviews_item">预览</view>
		 				<view class="cv_item_settingviews_item">删除</view>
		 			</view> -->
		 		</view>
		 		<view class="line"></view>
		 	</view>
		 </view>
		
	</view>
</template>

<script>
	var _this;
	export default {
		data() {
			return {
				showWorkExperience: false,
				showWorkExperienceNumb: 2,
				loginKey:'',
				showModal:true,
				dataInfo: Object,
			};
		},
		
		onShow() {
			this.getuserInfo()
		},
		onLoad() {
			_this = this
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
							console.log("have loginkey");
							this.showModal = false;
							this.getResumeInfo();
						} else {
							uni.removeStorageSync('loginKey');
							uni.removeStorageSync('userId');
							uni.removeStorageSync('isFill');
							this.showModal = true;
							this.showLoginModal();
						}
					})
					
				}else{
					this.showModal = true;
					this.showLoginModal();
				}
			},
			
			getResumeInfo(){
				var loginkey = uni.getStorageSync('loginKey');
				if (loginkey){
					this.loginKey = loginkey;
					this.$api.post('resume!ajaxGetResumeInfo.action', {
						loginKey: loginkey
					}).then(res => {
						if (res.res.status == 0) {
							this.dataInfo =  res.inf
						} else {
							uni.showToast({
								title:res.res.error
							})
						}
					})
					
				}else{
					this.showModal = true;
					this.showLoginModal();
				}
			},
			
			showLoginModal(){
				
					this.$showModal({
						title: "温馨提示",
						content: '登录过后才可体验求职小程序',
						showCancel: true,
						cancelText: "取消",
						cancelColor: "#000000",
						confirmText: "登录",
						confirmColor: "#3CC51F",
						success: function(res) {
							if (res.confirm) {
								uni.navigateTo({
									url: '../login/login'
								})
							} else {
							}
						}
					})
				
			},
			
			addcv(){
				var loginkey = uni.getStorageSync('loginKey');
				if (loginkey){
					this.loginKey = loginkey;
					this.$api.post('resume!ajaxCreateResume.action', {
						loginKey: loginkey
					}).then(res => {
						if (res.res.status == 0) {
							 uni.navigateTo({
							 	url:'./cvsetting?id='+res.inf.id
							 })
						} else {
							uni.showToast({
								title:res.res.error
							})
						}
					})
					
				}else{
					this.showModal = true;
					this.showLoginModal();
				}
				
			},
			dowmViewClick() {
				this.showWorkExperience = !this.showWorkExperience
				if (this.showWorkExperience == true) {
					this.showWorkExperienceNumb = this.data.workExperience.length
				} else {
					this.showWorkExperienceNumb = 2
				}
				console.log('this.showWorkExperience = ' + this.showWorkExperienceNumb);
			},


			wantjobClick() {
				uni.navigateTo({
					url: '../mine/jobManage'
				})
			},
			
			showcv(id){
				uni.navigateTo({
					url:'./showcv?id='+id
				})
			},
			seettingCv(id){
				uni.navigateTo({
					url:'./cvsetting?id='+id
				})
			},
			
			removeCv(id){
				
				uni.showModal({
					 title: '提示',
					    content: '确认删除该简历？',
					    success: function (res) {
					        if (res.confirm) {
					           var loginkey = uni.getStorageSync('loginKey');
					           _this.$api.post('resume!ajaxDeleteResume.action', {
					           	loginKey: loginkey,
					           	resumeId:id
					           }).then(res => {
					           	if (res.res.status == 0) {
					           		_this.getResumeInfo()
					           	} else {
					           		uni.showToast({
					           			title:res.res.error
					           		})
					           	}
					           })
					        } else if (res.cancel) {
					            console.log('用户点击取消');
					        }
					    }
					
				})
				
					
					
				
			},
			
			userMsg(){
				uni.navigateTo({
					url:'./basicInformation'
				})
				
			}
		}
	}
</script>

<style lang="scss">
	.topView {
		padding: 10px;
		font-size: 12px;
		background-color: #FFFFFF;

		.topView_nameView {
			display: flex;


			.topView_nameView_name {
				font-size: 16px;
			}

			.topView_nameView_xb {
				color: #999999;
				font-size: 12px;
				margin-top: 5px;
				margin-left: 3px;
				flex: 1;
			}
		}

		.topView_msg {
			color: #333333;
			height: 25px;
			line-height: 25px;

		}

		.topView_item {
			display: flex;
			height: 25px;
			align-items: center;

			.topView_item_img {
				width: 10px;
				height: 10px;
				background-color: #000000;
			}

			.topView_item_text {
				font-size: 12px;
				margin-left: 10px;
				line-height: 25px;
				flex: 1;
			}
		}

	}
	.view_item {
		display: flex;
		height: 25px;
		align-items: center;
		color: #333333;
	
		.view_item_img {
			width: 10px;
			height: 10px;
		}
	
		.view_item_text {
			font-size: 12px;
			margin-left: 10px;
			line-height: 25px;
			flex: 1;
		}
	}

	.rightBtn {
		width: 12px;
		height: 12px;
	}



	.line {
		margin: 10px 0px 0px 0px;
		height: 1px;
		background-color: #eceff3;
	}
	
	.mycv{
		padding: 20px 10px 0px 10px;
		.cv_title{
			font-size: 12px;
			color: #666666;
			margin-bottom: 10px;
		}
		
		.cv_item{
			display: flex;
			align-items: center;
			margin: 5px 0px 0px 0px;
			.cv_item_point{
				width: 5px;
				height: 5px;
				border-radius: 2.5px;
				background-color: #e8654b;
			}
			.cv_item_text{
				font-size: 12px;
				margin-left: 10px;
				flex: 1;
			}
			.cv_item_settingviews{
				display: flex;
				align-items: center;
				justify-content: center;
			
				.cv_item_settingviews_item{
					padding: 5px;
					color: #e8654b;
					font-size: 12px;
				}
			}
			
		}
	}
	
	.plateItemview_titleView {
		display: flex;
		align-items: center;
		height: 30px;
	
		.plateItemview_titleView_title {
			font-size: 12px;
			color: #666666;
			flex: 1;
		}
	
		.plateItemview_titleView_mes {
			width: 40px;
			height: 15px;
			line-height: 15px;
			font-size: 12px;
			margin-left: 5px;
			color: #e8654b;
			border-radius: 7.5px;
			border: 1px solid #e8654b;
			text-align: center;
		}
	
		.plateItemview_titleView_noneview {
			flex: 1;
		}
	}

</style>
