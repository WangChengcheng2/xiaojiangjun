<template>
	<view>
		<view class="headerWrap">
			<view class="header">
				<input class="ipt" type="text" value="" @input="find" placeholder="搜索您要找的门店名称" />
				<image :src="imgSrc+'public/fdj1.png'" class="img"></image>
			</view>
		</view>
		<view class="nav">
			<view class="tit1">综合排序</view>
			<view class="tit2">距离</view>
		</view>
		<view class="content">
			<view class="itemView" v-for="(item ,index) in liststores" :key="index">
				<image :src="item.logo" class="img"></image>
				<view class="rightBox">
					<view class="title">{{item.storename}}</view>
					<view class="center">
						<view v-for="(item ,index) in item.store_services" :key="index">{{item.service_name}}</view>
					</view>
					<view class="distance">{{item.distance}}km</view>
				</view>
				<image class="threedot" :src="imgSrc+'public/threeDot.png'"></image>
			</view>
		</view>
	</view>
</template>

<script>
	import { Request } from '../../public/utils.js'
	export default {
		data() {
			return {
				imgSrc: this.$store.state.imgSrc,
				liststores:[],
				longitude:'',
				latitude:''
			}
		},
		onLoad() {
			this.getLocation();
			
		},
		methods: {
			getLocation(){
				const that = this;
				wx.getLocation({
					type: 'gcj02 ',
					success (res) {
						that.longitude = res.longitude
						that.latitude = res.latitude
						that.getStores();
					}
				})
			},
			getStores(findkey=''){
				const that = this;
				Request(
					'store.get_stores',
					{
						lng:that.longitude,
						lat:that.latitude,
						findkey:findkey
					}
				).then((res)=>{
					console.log(res)
					this.liststores = res.data.list
					// 成功方法
				})
				.catch((res)=>{
					// 失败方法
				})
			},
			find(e){
				this.getStores(e.detail.value)
			}
		}
	}
</script>

<style lang="scss">
	page{
		background-color: #f6f6f6;
	}
.headerWrap{
	background-color: white;
	padding: 20rpx;
	.header{
		background-color: #f8f8f8;
		position: relative;
		width: 710rpx;
		border-radius: 40rpx;
		height: 80rpx;
		.ipt{
			width: 710rpx;
			height: 80rpx;
			background-color: #f2f2f2;
			border-radius: 40rpx;
			border: solid 1rpx #e3e3e3;
			line-height: 80rpx;
			font-family: PingFang-SC-Regular;
			font-size: 28rpx;
			font-weight: normal;
			font-stretch: normal;
			color: #999999;
			padding-left: 101rpx;
			padding-right: 29rpx;
			box-sizing: border-box;
			margin: 0 auto;
		}
		.img{
			position: absolute;
			width: 40rpx;
			height: 33rpx;
			left:29rpx;
			top: 23rpx;
		}
	}
}
//导航
.nav{
	display: flex;
	padding: 24rpx 30rpx;
	background-color: white;
	.tit1{
		font-family: PingFang-SC-Bold;
		font-size: 30rpx;
		font-weight: normal;
		font-stretch: normal;
		color: #1db728;
	}
	.tit2{
		font-family: PingFang-SC-Medium;
		font-size: 30rpx;
		font-weight: normal;
		font-stretch: normal;
		color: #999999;
		margin-left: 78rpx;
	}
}
//列表
.content{
	margin-top: 20rpx;
	.itemView{
		width: 750rpx;
		height: 202rpx;
		background-color: #ffffff;
		padding: 21rpx 14rpx;
		box-sizing: border-box;
		position: relative;
		display: flex;
		.img{
			width: 160rpx;
			height: 160rpx;
			border-radius: 10rpx;
		}
		.rightBox{
			margin-left: 20rpx;
			.title{
				font-family: PingFang-SC-Bold;
				font-size: 32rpx;
				font-weight: normal;
				font-stretch: normal;
				color: #333333;
			}
			.center{
				display: flex;
				margin-top: 20rpx;
				margin-bottom: 34rpx;
				view{
					width: 66rpx;
					height: 32rpx;
					background-color: #f4f4f4;
					border-radius: 16rpx;
					font-family: PingFang-SC-Medium;
					font-size: 22rpx;
					margin-right: 10rpx;
					font-weight: normal;
					font-stretch: normal;
					text-align: center;
					color: #474747;
				}
			}
			.distance{
				font-family: PingFang-SC-Medium;
				font-size: 28rpx;
				font-weight: normal;
				font-stretch: normal;
				color: #666666;
			}
		}
		.threedot{
			position: absolute;
			right: 30rpx;
			top: 98rpx;
			width: 34rpx;
			height: 8rpx;
		}
	}
}
</style>
