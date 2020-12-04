<template>
	<view>
		<textarea disabled=true auto-height=true class="toptext" :value="dataInf.invitationCardContent " placeholder="" />
		<view class="titleView">
			<view class="titleView_title">
				面试职位
			</view>
		</view>
		
		
		<view class="cell_bk">
			<view class="cell_mainV">
				<view class="cell_content" >
					<view class="cell_titleview" >
						<view class="cell_titleview_title">
							{{dataInf.position.name}}
						</view>
						<view class="cell_titleview_salary" >
							{{dataInf.position.salaryRequirement}}
						</view>
					</view>
					
					<view class="cell_msgViews" >
						<view class="cell_msg"  >
							{{dataInf.position.location+'&nbsp|&nbsp'+dataInf.position.expRequirement+'&nbsp|&nbsp'+dataInf.position.educationRequirement}}
						</view>
					</view>
					
					<view class="cell_bottomview" >
						<view class="cell_bottomview_company" >
							{{dataInf.position.companyName}}
						</view>
						<view class="cell_bottomview_time" >
							{{dataInf.position.upDate}}
						</view>
					</view>
			
				</view>
			</view>
		</view>
		
		
		
		<view class="" style="height: 50px ; width: 100%;">
			<view class="bottom_btns">
				<view class="bottom_btns_btn" @click="obok(2)">
					拒绝
				</view>
				<view class="bottom_btns_btn_callme" @click="obok(1)">
					接受
				</view>		
			</view>
		</view>
		
		
	</view>
</template>

<script>
	export default{
		data(){
			return{
				dataInf:'',
				invittationId:'',
				delta:1
			};
		},
		
		onLoad(e) {
			//ajaxGetInvitationCardInfo
			this.invittationId = e.id
			this.delta = e.delta
			
			console.log('delta = '+this.delta);
			this.getData()
		},
			
		methods:{
				
			getData(){
				
					
					var loginkey = uni.getStorageSync('loginKey');
					if (loginkey) {
						this.loginKey = loginkey;
						this.$api.post('qzPosition!ajaxGetInvitationCardInfo.action', {
							loginKey: loginkey,
							id:this.invittationId
						}).then(res => {
							if (res.res.status == 0) {
								this.dataInf = res.inf
							} else {
				
							}
						})
				
					} else {}
				
			},
			
			obok(e){
				var _this = this
				var loginkey = uni.getStorageSync('loginKey');
			
				if (loginkey) {
					this.loginKey = loginkey;
					this.$api.post('qzPosition!ajaxUpdateInterviewInvitationCardStatus.action', {
						loginKey: loginkey,
						id:this.dataInf.id,
						interviewInvitationStatus:e
					}).then(res => {
						if (res.res.status == 0) {
							uni.showToast({
								title:e===1?'已接受':'已拒绝',
								success() {
									console.log('de='+_this.delta);
										if(_this.delta == 1){
											uni.navigateBack({
												delta:1
											})
										}else{
											uni.navigateBack({
												delta:2
											})
										}
										
										
								}
							})
						} else {
							uni.showToast({
								title:res.res.errMsg
							})	
						}
					})
								
				} else {}
			},
			
		}
	}
</script>

<style lang="scss">
	
	.toptext{
		padding: 10px;
		color: #000;
		font-size: 14px;
		width: 100%;

	}
	
	.titleView{
		background-color: #f5f7f8;
		.titleView_title{
			padding: 3px 10px;
			color: #333333;
			font-size: 12px;
		}
	}
	
	.cell_bk{
		background-color: #f5f7f8;
	}
	.cell_mainV {
		display: flex;
		// align-items: center;
		padding: 10px;
		background-color: #FFFFFF;
		
	}
	
	
	
	
	.cell_content {
		flex: 1;
	}
	
	.cell_titleview {
		display: flex;
		align-items: center;
		margin-top: 10px;
	
		.cell_titleview_title {
			font-size: 14px;
	
			flex: 1;
		}
	
		.cell_titleview_salary {
			font-size: 14px;
	
			color: #e8654b;
		}
		
		.cell_isDelivery{
			color: #CCCCCC;
		}
	
	}
	
	.cell_msgViews{
		display: flex;
		align-items: center;
		.cell_msg {
			font-size: 12px;
			color: #333333;
			flex: 1;
		}
		
		.cell_status{
			font-size: 12px;
			color: #CCCCCC;
		}
		
		.cell_isDelivery{
			color: #CCCCCC;
		}
	}
	
	.cell_bottomview {
		display: flex;
		align-items: center;
	
		.cell_bottomview_company {
	
			flex: 1;
			font-size: 12px;
			color: #333333;
		}
	
		.cell_bottomview_time {
			font-size: 12px;
	
		}
		
		.cell_isDelivery{
			color: #CCCCCC;
		}
	
	}
	
	.bottom_btns{
		position: fixed;
		display: flex;
		bottom:10px;
		align-items: center;
		height: 50px;
		width: 100%;
		.bottom_btns_btn{
			margin: 0px 10px;
			flex: 1;
			border: 1px solid #e8654b;
			border-radius: 5px;
			background-color: #fddfd6;
			color: #e8654b;
			font-size: 14px;
			text-align: center;
			line-height: 30px;
			height: 30px;
		}
		
		.bottom_btns_btn_callme{
			margin: 0px 10px;
			flex: 1;
			border: 1px solid #e8654b;
			border-radius: 5px;
			background-color: #e8654b;
			color: #FFFFFF;
			font-size: 14px;
			text-align: center;
			line-height: 30px;
			height: 30px;
		}
	}
	
</style>
