<template>
	<view>
		<view class="top_view">
			<view class="top_view_title">
				管理求职意向 <text class="top_view_ftitle">( <text style="color: #e8654b;">{{dataArr.length}}</text>/3) </text>
			</view>
			<view class="top_view_msg">
				我们会根据你的"求职意向"在首页展示合适的职位
			</view>
		</view>


		<view class="selectView" @tap="handleTap('picker31')">
			<view class="titleLabel">求职状态</view>
			<view class="selectView_btn">{{dataInf.selJobStatusContent.length>0?dataInf.selJobStatusContent:'月内到岗'}}{{zwf}}⟩</view>
		</view>
		<lb-picker :props="myProps" ref="picker31" mode="multiSelector" level='1' :list="typelist" @confirm="handleConfirm">
		</lb-picker>
		<view class="line">	</view>
		<!-- <lb-picker ref="picker31"
		  v-model="type"
		  mode="selector"
		  :list="typelist">
		  </lb-picker>
 -->

		<view class="list">
			<view v-for="(item,index) in dataArr" :key="index">
				<view class="cell">
					<view class="cell_content">
						<view class="cell_content_title">
							{{item.expectedPosition}}
						</view>
						<view class="cell_content_detail">
							{{item.expectedSalary+'&nbsp|&nbsp'+item.workLocation+'&nbsp|&nbsp'+item.workNature}}
						</view>
						
						<view class="cell_content_detail">
							{{item.companyNatures}}
						</view>
					</view>
					
					<image class="cell_endBtn" src="../../static/cv/bj.png" mode="" @click="editExpected(item)"> </image>
				</view>
				<view class="line">
					
				</view>
			</view>
		</view>
		
		
		<view class="bottom_view" >
			<view class="bottom_btn" @click="addClick()">
				添加求职意向
			</view>
		</view>

	</view>
</template>

<script>
	export default {

		data() {
			return {
				
				myProps: {
					label: 'content',
					value: 'id',
					children: 'child'
				},
				typelist: ['离职，随时到岗', '在职，月内到岗', '在职，考虑机会','在职，暂不考虑'],
				type: '月内到岗',
				typeId:'',
				zwf:'\u0020',
				id:'',
				dataInf:{
					selJobStatusContent:''
				},
				dataArr:[],
			};
		},
		
		onLoad(e) {
			this.id = e.id
			this.getdownList()
		},
		
		onShow() {
			this.getMes()
		},
		
		

		methods: {
			handleTap(picker) {
				this.$refs[picker].show()
			},
				
			handleConfirm(e){
				
				this.dataInf.selJobStatusContent = e.value.map(item => item).join('-');
				for (var i = 0; i <  e.item.length; i++) {
					if(i==0){
						console.log('positionInfoId='+e.item[i].id);
						this.typeId = e.item[i].id
						this.upstatus(this.typeId)
					}
				}
			},
			
			upstatus(id){
				//ajaxUpdateResumeJobStatus
				var loginkey = uni.getStorageSync('loginKey');
				this.$api.post('resume!ajaxUpdateResumeJobStatus.action', {
					loginKey: loginkey,
					resumeId:this.id,
					jobStatusId:id
				}).then(res => {
					if (res.res.status == 0) {
					this.typelist = res.inf.jobWantedStatusArr
						
					} else {
						uni.showToast({
							title:res.res.error
						})
					}
				})
			},
			
				
			editExpected(item){
				uni.navigateTo({
					url:'./addJobIntention?id='+this.id+'&wantedIntentionId='+item.id
				})
			},
			addClick(){
				
					
				if(this.dataArr.length>4){
					uni.navigateTo({
						url:'./addJobIntention?id='+this.id
					})
					
				}else{
					uni.showToast({
						title:'最多可添加3条！'
					})
				}
				
			},
			
			getdownList(){
				var loginkey = uni.getStorageSync('loginKey');
				this.$api.post('resume!ajaxGetJobStatusDropdownInfo.action', {
					loginKey: loginkey
				}).then(res => {
					if (res.res.status == 0) {
					this.typelist = res.inf.jobWantedStatusArr
						
					} else {
						uni.showToast({
							title:res.res.error
						})
					}
				})
			},
			getMes(){
				var loginkey = uni.getStorageSync('loginKey');
				this.$api.post('resume!ajaxGetWantedIntentionList.action', {
					loginKey: loginkey,
					resumeId:this.id
				}).then(res => {
					if (res.res.status == 0) {
						this.dataArr = res.inf.arr
						this.dataInf = res.inf
					} else {
						uni.showToast({
							title:res.res.error
						})
					}
				})
			},
			
		}
	}
</script>

<style lang="scss">
	.top_view {
		padding: 10px 10px;
		margin-top: 10px;

		.top_view_title {
			font-size: 14px;
		}

		.top_view_ftitle {
			font-size: 12px;
			color: #666666;
		}

		.top_view_msg {
			font-size: 12px;
			height: 30px;
			line-height: 30px;
			color: #666666;
		}

	}

	.company {
		display: flex;
		margin: 0px 10px 10px 10px;
		border-radius: 5px;
		background-color: #f5f7f8;
		font-size: 14px;
		color: #666666;

		.company_content {
			flex: 1;
			padding: 5px;

		}

		.company_imgView {
			width: 30px;
			height: 30px;
			background-color: #e8654b;
			border-top-right-radius: 5px;
			border-bottom-right-radius: 5px; //右下角

			display: flex;
			align-items: center;
			justify-content: center;

			.company_img {
				width: 8px;
				height: 5px;
			}
		}

	}

	.titleLabel {
		font-size: 14px;
		padding: 10px 10px;
		line-height: 20px;
		flex: 1;
	}
	
	.list{
		padding: 10px;
		
		.cell{
			display: flex;
			align-items: center;
			.cell_content{
				flex:1;
				.cell_content_title{
					font-size: 14px;
					height: 30px;
					line-height: 30px;
				}
				
				.cell_content_detail{
					font-size: 12px;
					height: 30px;
					line-height: 30px;
				}
			}
			.cell_endBtn{
				width: 20px;
				height: 20px;
			}
			
		}
	}
	
	.line{
		margin: 0px;
		width: 100%;
		height: 1px;
		background-color: #e6e6e6;
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
	
	.selectView{
		display: flex;
		align-items: center;
		margin-bottom: 10px;
	}
	
	.selectView_btn{
		font-size: 12px;
		color: #666666;
		margin-right: 10px;
	}
	
</style>
