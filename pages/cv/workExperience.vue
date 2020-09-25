<template>
	<view>

		<view class="item_views">
			<view class="item_title">
				公司名称
			</view>
			<view class="item">
				<input class="item_content" type="text" value="" v-model="companyName" />
			</view>
		</view>




		<view class="item_views">
			<view class="item_title">
				职位名称
			</view>
			<view class="item" @tap="handleTap('picker333')">
				<view class="item_content">{{jobName}}</view>
				<view class="item_imgView">
					<image class="item_img" src="../../static/login/star3.png" mode=""></image>
				</view>
			</view>
		</view>
		<lb-picker ref="picker333" v-model="jobName" mode="selector" :list="typelist">
		</lb-picker>


		<view class="item_views">
			<view class="item_title">
				在职时间
			</view>
			<view class="item" @tap="handleTap('pickerdate1')">
				<view class="item_content">{{beginTime}}</view>
				<view class="item_imgView">
					<image class="item_img" src="../../static/login/star3.png" mode=""></image>
				</view>
			</view>
			<view class="item_title" style="margin-left: 5px;">
				-
			</view>
			<view class="item" @tap="handleTap('pickerdate2')">
				<view class="item_content">{{endTime}}</view>
				<view class="item_imgView">
					<image class="item_img" src="../../static/login/star3.png" mode=""></image>
				</view>
			</view>
		</view>
		<e-picker-plus ref="pickerdate1" @confirm="confirm1" mode="YM" :endRule=currentDate> </e-picker-plus>
		<e-picker-plus ref="pickerdate2" @confirm="confirm2" mode="YM" :endRule=currentDate> </e-picker-plus>
		<!-- <lb-picker ref="picker335" v-model="type" mode="selector" :list="typelist"></lb-picker> -->

		<view class="item_views">
			<view class="item_title">
				所属行业
			</view>
			<view class="item" @tap="handleTap('picker3366')">
				<view class="item_content">{{industry}}</view>
				<view class="item_imgView">
					<image class="item_img" src="../../static/login/star3.png" mode=""></image>
				</view>
			</view>
		</view>
		<lb-picker ref="picker3366" v-model="industry" mode="selector" :list="typelist">
		</lb-picker>

		<view class="item_views">
			<view class="item_title">
				当前月薪
			</view>
			<view class="item">
				<input class="item_content" type="text" value="" v-model="salary" placeholder="--k" />
			</view>
		</view>


		<view class="item_views">
			<view class="item_title">
				工作描述
			</view>
		</view>
		<textarea class="desView" type="text" value="" v-model="workDes" />


		<view class="item_views">
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
				companyName: '',
				jobName: '',
				beginTime: '',
				endTime: '至今',
				industry: '',
				salary: '',
				workDes: '',
				typelist: ['1', '2', '3', '4'],
				type: '',
				switchValue: false,
				dataDic:{},
				id:null
			};
		},


		onLoad(e) {
			if(e.data){
				if(e.data.length>0){
					let  dataDic =  JSON.parse(decodeURIComponent(e.data));
					this.dataDic = dataDic;
					this.companyName = this.dataDic.company;
					this.jobName =  this.dataDic.job;
					this.beginTime = this.dataDic.beginTime;
					this.endTime =this.dataDic.endTime;
					this.industry = this.dataDic.industry;
					this.salary = this.dataDic.salary;
					this.workDes = this.dataDic.detail;
					this.id = this.dataDic.id;
					this.switchValue = this.dataDic.private===1?true:false
				}
			}
			
		},
		methods: {
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
				console.log('this.switch = ' + this.switchValue);
			},
			confirm1(e) {
				this.beginTime = e.result
				console.log(e.result)
			},
			confirm2(e) {
				if(e.result === this.currentDate){
					this.endTime='至今'
				}else{
					this.endTime = e.result
				}
			},
			saveClick(){
				
			
				
				let pages = getCurrentPages();  //获取所有页面栈实例列表
				let nowPage = pages[ pages.length - 1];  //当前页页面实例
				let prevPage = pages[ pages.length - 2 ];  //上一页页面实例
				
				
				console.log("prevPage="+prevPage);
				if(this.id){
					//修改
					console.log("修改");
					
					let dic = {
						company:this.companyName,
						job:this.jobName,
						beginTime:this.beginTime,
						endTime:this.endTime,
						industry:this.industry,
						salary:this.salary,
						detail:this.workDes,
						id:this.id,
						private:this.switchValue?1:0
					}
					
					
					// prevPage.$vm.data.workExperience[this.id-1].company=this.companyName;
					// prevPage.$vm.data.workExperience[this.id-1].job=this.jobName,
					// prevPage.$vm.data.workExperience[this.id-1].beginTime=this.beginTime,
					// prevPage.$vm.data.workExperience[this.id-1].endTime=this.endTime,
					// prevPage.$vm.data.workExperience[this.id-1].industry=this.industry,
					// prevPage.$vm.data.workExperience[this.id-1].salary=this.salary,
					// prevPage.$vm.data.workExperience[this.id-1].detail=this.workDes,
					// prevPage.$vm.data.workExperience[this.id-1].id=prevPage.$vm.data.workExperience.length==0?1:prevPage.$vm.data.workExperience.length+1,
					// prevPage.$vm.data.workExperience[this.id-1].private=this.switchValue?1:0
					prevPage.$vm.data.workExperience.splice(this.id-1,1,dic);
				}else{
				   console.log("添加");
					prevPage.$vm.data.workExperience.push({
						company:this.companyName,
						job:this.jobName,
						beginTime:this.beginTime,
						endTime:this.endTime,
						industry:this.industry,
						salary:this.salary,
						detail:this.workDes,
						id:prevPage.$vm.data.workExperience.length==0?1:prevPage.$vm.data.workExperience.length+1,
						private:this.switchValue?1:0
					})
				}
				uni.navigateBack({
					
				})
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
		height: 60px;
		font-size: 14px;
		color: #666666;
	}

	.sw {
		transform: scale(0.7, 0.7);
	}

	.timefont {
		font-size: 12px;
	}
</style>
