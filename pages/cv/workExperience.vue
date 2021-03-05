<template>
	<view>

		<view class="item_views">
			<view class="item_title">
				公司名称
			</view>
			<view class="item">
				<input class="item_content" style="padding: 5px;" type="text" value="" v-model="dataInf.companyName" />
			</view>
		</view>




		<view class="item_views">
			<view class="item_title">
				职位名称
			</view>
			<view class="item" @tap="handleTap('picker333')">
				<view class="item_content">{{dataInf.positionName}}</view>
				<view class="item_imgView">
					<image class="item_img" src="../../static/login/star3.png" mode=""></image>
				</view>
			</view>
		</view>
		<!-- <lb-picker ref="picker333" v-model="jobName" mode="selector" :list="typelist"> -->
		<lb-picker ref="picker333" mode="multiSelector" level='2' :list="dataInf.positionArr" @confirm="handleConfirm1">
		</lb-picker>


		<view class="item_views">
			<view class="item_title">
				在职时间
			</view>
			<view class="item" @tap="handleTap('pickerdate1')">
				<view class="item_content">{{dataInf.entryDate}}</view>
				<view class="item_imgView">
					<image class="item_img" src="../../static/login/star3.png" mode=""></image>
				</view>
			</view>
			<view class="item_title" style="margin-left: 5px;">
				-
			</view>
			<view class="item" @tap="handleTap('pickerdate2')">
				<view class="item_content">{{dataInf.resignationDate}}</view>
				<view class="item_imgView">
					<image class="item_img" src="../../static/login/star3.png" mode=""></image>
				</view>
			</view>
		</view>
		<!-- <e-picker-plus ref="pickerdate1" @confirm="confirm1" mode="YM" :endRule=currentDate> </e-picker-plus>
		<e-picker-plus ref="pickerdate2" @confirm="confirm2" mode="YM" :endRule=currentDate> </e-picker-plus> -->
		<bory-dateTimePicker ref='pickerdate1' :indicatorStyle='indicatorStyle' :type='type' :datestring='workTime' @change='confirm1'></bory-dateTimePicker>
		<bory-dateTimePicker ref='pickerdate2' :indicatorStyle='indicatorStyle' :type='type' :datestring='workTime' @change='confirm2'></bory-dateTimePicker>

		<view class="item_views">
			<view class="item_title">
				所属行业
			</view>
			<view class="item" @tap="handleTap('picker3366')">
				<view class="item_content">{{dataInf.industry}}</view>
				<view class="item_imgView">
					<image class="item_img" src="../../static/login/star3.png" mode=""></image>
				</view>
			</view> 
		</view>
		<lb-picker ref="picker3366" mode="multiSelector" level='2' :list="dataInf.industryArr" @confirm="handleConfirm">
		</lb-picker>

		<view class="item_views">
			<view class="item_title">
				当前月薪
			</view>
			<view class="item">
				<input class="item_content" style="padding: 5px;" type="text" value="" v-model="dataInf.monthSalary" />
				<view class="item_placeholder">/k</view>
			</view>
		</view>


		<view class="item_views">
			<view class="item_title">
				工作描述
			</view>
		</view>
		<textarea class="desView" cursor-spacing="0" type="text" maxlength="-1" value="" v-model="dataInf.workDescription" />


		<view class="item_views" style="margin-bottom: 60px;">
			<view class="item_title">
				对这家公司隐藏我的信息
			</view>
			<switch class="sw" name="switch" @click="switchClick()" :checked="switchValue" />

		</view>

		<view class="bottom_view">
			<view class="bottom_view_qd" @click="saveClick()">保存</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			const currentD = this.getDate({
				format: true
			})
			return {
				currentDate: currentD,
				switchValue: false,
				type:'year-month',
				cvid:'',
				workExpId:'',
				dataInf:{
					companyName: '',
					resumeId:'',//简历id
					positionInfoId:'',//职位名称id(二级列表中的id)
					entryDate:'',//入职日期
					resignationDate:'',//离职日期
					industryInfoId:'',//所属行业id(二级列表中的id)
					selIndustryInfoId:'',
					monthSalary:'',//月薪
					workDescription:'',
					industry: '',
					industrystr:'', //所属行业文字 显示用
					positionName:'',//职位名称
					isShield:'0',//是否屏蔽公司，1表示屏蔽，0表示不屏蔽
					positionArr:[],
					industryArr:[],
						
				}
			
			};
		},


		onLoad(e) {
			if(e.id){
				this.cvid = e.id
			}
				
			if(e.workExpId){
				this.workExpId = e.workExpId
				console.log('this.workExpId='+e.workExpId);
				this.getMes(e.workExpId);
			}
			
			this.getdownList()
	
		},
		computed: {
		
			indicatorStyle() {
			                return {
			                    background: 'rgba(15, 128, 255, 0.4)',
			                    height: '40px',
			                };
			            },
			
		
		},
		methods: {
			
			handleConfirm(e){

				this.dataInf.industry = e.value.map(item => item).join('-');
				for (var i = 0; i < e.item.length; i++) {
					if(i==0){
						
					}else{
						console.log('industryInfoId='+e.item[i].value);
						this.dataInf.industryInfoId = e.item[i].value
					}
				}
			},
			
			handleConfirm1(e){
				
					this.dataInf.positionName = e.value.map(item => item).join('-');
					for (var i = 0; i <  e.item.length; i++) {
						if(i==0){
							
						}else{
							console.log('positionInfoId='+e.item[i].value);
							this.dataInf.positionInfoId = e.item[i].value
						}
					}
				
			},
			
			getdownList(){
				var loginkey = uni.getStorageSync('loginKey');
				this.$api.post('resume!ajaxGetWorkExpDropdownInfo.action', {
					loginKey: loginkey
				}).then(res => {
					if (res.res.status == 0) {
						this.dataInf.positionArr = res.inf.positionArr;
						this.dataInf.industryArr = res.inf.industryArr;
						
					} else {
						uni.showToast({
							title:res.res.error
						})
					}
				})
			},
			getMes(workid){
				var loginkey = uni.getStorageSync('loginKey');
				this.$api.post('resume!ajaxGetWorkExperienceInfo.action', {
					loginKey: loginkey,
					workExpId:workid
				}).then(res => {
					if (res.res.status == 0) {
						this.dataInf = res.inf
						this.dataInf.resumeId = this.cvid
						this.switchValue = res.inf.isShield === 1?true:false
						this.dataInf.positionInfoId = res.inf.selPositionInfoId
						this.dataInf.industryInfoId = res.inf.selIndustryInfoId
					} else {
						uni.showToast({
							title:res.res.error
						})
					}
				})
			},
			
			getDate(type) {
				const date = new Date();

				let year = date.getFullYear();
				let month = date.getMonth() + 1;
				let day = date.getDate();

				if (type === 'start') {
					year = year - 60;
				} else if (type === 'end') {
					year = year + 2;
				}
				month = month > 9 ? month : '0' + month;;
				day = day > 9 ? day : '0' + day;
				let ztime = `${year}-${month}`
				return ztime;
			},

			handleTap(picker) {
				this.$refs[picker].show()
			},
			switchClick() { 
				this.switchValue = !this.switchValue;
				if(this.switchValue === true){
					this.dataInf.isShield = '1'
				}else{
					this.dataInf.isShield='0'
				}
				console.log('this.switch = ' + this.switchValue);
			},
			confirm1(e) {
				this.dataInf.entryDate = e
			},
			confirm2(e) {
				if(e === this.currentDate){
					this.dataInf.resignationDate='至今'
				}else{
					this.dataInf.resignationDate = e
				}
			},
			saveClick(){
			
			   
			   if(this.workExpId.length>0){
				   var loginkey = uni.getStorageSync('loginKey');
				   let  workExpInf = JSON.stringify(this.dataInf)
				   this.$api.post('resume!ajaxUpdateWorkExperience.action', {
				   	loginKey: loginkey,
					workExpId:this.workExpId,
				   	workExpInfo:workExpInf
				   }).then(res => {
				   	if (res.res.status == 0) {
				   		uni.showToast({
				   			title:'保存成功',
				   						success() {
				   							uni.navigateBack({
				   								
				   							})
				   						}
				   		})
				   	} else {
				   		uni.showToast({
				   			title:res.res.error
				   		})
				   	}
				   })
			   }else{
				   var loginkey = uni.getStorageSync('loginKey');
				   let dict = {
				   				   resumeId:this.cvid,
				   				   companyName:this.dataInf.companyName,
				   				   positionInfoId:this.dataInf.positionInfoId,
				   				   entryDate:this.dataInf.entryDate,
				   				   resignationDate:this.dataInf.resignationDate,
				   				   industryInfoId:this.dataInf.industryInfoId,
				   				   monthSalary:this.dataInf.monthSalary,
				   				   workDescription:this.dataInf.workDescription,
				   				   isShield:this.dataInf.isShield
				   }
				   let  workExpInf = JSON.stringify(dict)
				   this.$api.post('resume!ajaxAddWorkExperience.action', {
				   	loginKey: loginkey,
				   	workExpInfo:workExpInf
				   }).then(res => {
				   	if (res.res.status == 0) {
				   		uni.showToast({
				   			title:'保存成功',
				   						success() {
				   							uni.navigateBack({
				   								
				   							})
				   						}
				   		})
				   	} else {
				   		uni.showToast({
				   			title:res.res.error
				   		})
				   	}
				   })
			   }
			   
			   
			}
					
				
		}
	
	}
</script>

<style lang="scss">
	.item_views {
		margin-top: 10px;
		display: flex;
		align-items: center;
		padding: 10px;

		.item_title {
			margin-right: 10px;
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
				padding: 0px 5px;
				line-height: 30px;

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
			.item_placeholder{
				width: 30px;
				height: 30px;
				line-height: 30px;
				border-top-right-radius: 5px;
				border-bottom-right-radius: 5px; //右下角
				font-size: 12px;
				text-align: center;
			}

		}
	}
	
	.input_view{
		height: 30px;
		line-height: 30px;
	}

	.bottom_view {
		position: fixed;
		bottom: 0px;
		height: 50px;
		display: flex;
		justify-content: center;
		width: 100%;
		z-index: 999;
	
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
			margin: 10px 10px 10px 10px;
			background-color: #e8654b;
			color: #FFFFFF;
			font-size: 17px;
			text-align: center;
			line-height: 30px;
	
		}
	}

	.desView {
		margin: 5px 10px;
		padding: 10px;
		width: calc(100vw - 40px);
		background-color: #f5f7f8;
		height: 400px;
		font-size: 14px;
		color: #666666;
		// z-index: 999;
	}

	.sw {
		transform: scale(0.7, 0.7);
	}

	.timefont {
		font-size: 12px;
	}
</style>
