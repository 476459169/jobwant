<template>
	<view class="">
		
		<view class="titleLabel">公司所在地</view>
		<view class="address_view">
			<view class="address_item" @tap="handleTap('picker22')">
				<view class="address_content">{{address.length>0?address.join('-'):''}}</view>
				<view class="address_img">
					<image class="add_itemImg" src="../../static/login/star3.png" mode=""></image>
				</view>
		
				<lb-picker ref="picker22" v-model="address" mode="multiSelector" :list="list" :level="3"
				>
				</lb-picker>
			</view>
		</view>
		
		<view class="contentView">
			<view class="title_view">简历状态</view>
			<view class="top_view">
				<view class="top_view_item" :class="[xlSelect == item?'top_view_select_color':'']" v-for="(item,index) in xlArr"
				 :key="index" @click="itemClick(item,1)">
					{{item}}
				</view>
			</view>
			
			<view class="title_view">工作经验</view>
			<view class="top_view">
				<view class="top_view_item" :class="[workSelect == item?'top_view_select_color':'']" v-for="(item,index) in worKArr"
				 :key="index" @click="itemClick(item,2)">
					{{item}}
				</view>
			</view>
			
			
			
			<view class="title_view">公司性质</view>
			<view class="top_view">
				<view class="top_view_item" :class="[companyTypeSelect == item?'top_view_select_color':'']" v-for="(item,index) in companyTypeArr"
				 :key="index" @click="itemClick(item,3)">
					{{item}}
				</view>
			</view>
			
			
			<view class="title_view">公司规模</view>
			<view class="top_view">
				<view class="top_view_item" :class="[companyNumbSelect == item?'top_view_select_color':'']" v-for="(item,index) in companyNumbArr"
				 :key="index" @click="itemClick(item,4)">
					{{item}}
				</view>
			</view>
			
			<view class="bottom_view">
				<view class="bottom_view_cz">重置</view>
				<view class="bottom_view_qd">确定</view>
			</view>
			
		</view>
		
	
	</view>
</template>

<script>
	
	import areaData from './area-data-min'
	export default{
		data(){
			return{
					xlArr: ['不限', '大专', '本科', '硕士', 'MBA/EMBA', '博士'],
					worKArr:['全部','无经验','1年以下','1-3年','3-5年','5-10年','10年以上'],
					companyTypeArr:['全部','国企','外商独资','合资','民营','股份制企业','上市公司','医院','药企'],
					companyNumbArr:['全部','20人以下','20-99人','100-299人','300-499人','500-999人','1000-9999人','10000人以上'],
					list:areaData,
					address:[],
					xlSelect:'不限',
					workSelect:'全部',
					companyTypeSelect:'全部',
					companyNumbSelect:'全部'
			}
		},
			
		methods:{
			handleTap (picker) {
							this.$refs[picker].show()
							// console.log("handleTap");
						},
			itemClick(item,ob){
				if(ob==1){
					this.xlSelect = item
				}else if(ob==2){
					this.workSelect = item
				}else if(ob==3){
					this.companyTypeSelect = item
				}else if(ob==4){
					this.companyNumbSelect = item
				}
				
			}
		}
		
	}
</script>

<style lang="scss">
	
	
	
	
	
	
	.address_view {
		display: flex;
	
		// padding: 5px;
		.address_item {
			display: flex;
			width: 100%;
			height: 30px;
			margin: 0px 15px;
			border-radius: 5px;
			background-color: #f5f7f8;
			color: #666666;
			font-size: 14px;
	
			.address_line {
				height: 30px;
				width: 2px;
				background-color: #FFFFFF;
			}
	
			.address_bt {
				width: 40px;
				text-align: center;
				line-height: 30px;
			}
	
			.address_content {
				flex: 1;
				line-height: 30px;
				padding-left: 5px;
			}
	
	
			.address_img {
				width: 30px;
				height: 30px;
				background-color: #e8654b;
				border-top-right-radius: 5px;
				border-bottom-right-radius: 5px; //右下角
				display: flex;
				align-items: center;
				justify-content: center;
				.add_itemImg{
					width: 8px;
					height: 5px;
				}
			}
	
		}
	
	}
	
	.titleLabel {
		font-size: 14px;
		padding: 10px 20px;
		margin-top: 20px;
		line-height: 15px;
	}
	
	.contentView{
		padding: 10px;
		
		.title_view {
			margin-top: 20px;
			line-height: 20px;
			font-size: 14px;
			margin-left: 10px;
		}
		
		.top_view {
			display: flex;
			flex-wrap: wrap;
			.top_view_item {
				background-color: #fddfd6;
				color: #8f9291;
				font-size: 13px;
				border-radius: 5px;
				width: 30%;
				height: 25px;
				margin-top: 10px;
				margin-left: 10px;
				padding: 3px 0px;
				text-align: center;
				line-height: 25px;
				letter-spacing:2px;
			}
			
			.top_view_select_color {
				color: #e8654b;
			}
		}
	}
	
	.bottom_view {
		margin-top: 30px;
		height: 50px;
		display: flex;
		justify-content: center;
		width: 100%;
	
		.bottom_view_cz {
			border: 2px solid #e8654b;
			background-color: #fbc9bc;
			color: #e8654b;
			font-size: 14px;
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
			margin: 10px 10px 10px 0px;
			background-color: #e8654b;
			color: #FFFFFF;
			font-size: 14px;
			text-align: center;
			line-height: 30px;
	
		}
	}
</style>
