<template>
	<view class="">
		<u-navbar title-color="#fff" back-icon-color="#ffffff" 
			:is-fixed="isFixed" :is-back="isBack" 
			:background="background" 
			:back-text-style="{color: '#fff'}" :title="title" 
			:back-icon-name="backIconName" :back-text="backText"
		>
		<view class="slot-wrap" v-if="useSlot">
			<view class="search-wrap" v-if="search">
				<!-- 如果使用u-search组件，必须要给v-model绑定一个变量 -->
				<u-search  @custom="customclick" v-model="keyword" action-text="搜索" :show-action="showAction" height="56" :action-style="{color: '#fff'}"></u-search>
			</view>
			<view class="navbar-right" v-if="rightSlot">
				<view class="message-box right-item">
					<u-icon name="chat" size="38"></u-icon>
					<u-badge count="18" size="mini" :offset="[-15, -15]"></u-badge>
				</view>
				<view class="dot-box right-item">
					<u-icon name="calendar-fill" size="38"></u-icon>
					<u-badge size="mini" :is-dot="true" :offset="[-6, -6]"></u-badge>
				</view>
			</view>
			<view class="map-wrap" v-if="custom">
				<u-icon name="map" color="#ffffff" size="24"></u-icon>
				<text class="map-wrap-text"></text>
				<u-icon name="arrow-down-fill" color="#ffffff" size="22"></u-icon>
			</view>
		</view>
		<view class="navbar-right" slot="right" v-if="slotRight">
				<view class="message-box right-item">
					<u-icon name="chat" size="38"></u-icon>
					<u-badge count="18" size="mini" :offset="[-15, -15]"></u-badge>
				</view>
				<view class="dot-box right-item">
					<u-icon name="calendar-fill" size="38"></u-icon>
					<u-badge size="mini" :is-dot="true" :offset="[-6, -6]"></u-badge>
				</view>
		</view>
	</u-navbar>
	<scroll-view scroll-y style="height: 100%;width: 100%;" @scrolltolower="reachBottom">
						<view class="page-box">
							<view class="order" v-for="(item, index) in dataList" :key="item.id">
								<view class="top">
									<view class="left">
										<view class="store">{{ item.classfullname }}</view>
									</view>
									<view class="right">{{ renderRecycle(item) }}</view>
								</view>
								<view class="item" >
									<view class="left"><image src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1599994418340&di=8763d31be74a803917fa134fa743afdb&imgtype=0&src=http%3A%2F%2Fa2.att.hudong.com%2F36%2F48%2F19300001357258133412489354717.jpg" mode="aspectFill"></image></view>
									<view class="content">
										<view class="title u-line-2">型号:{{ item.model }}</view>
										<view class="type">编号:{{ item.uuid }}</view>
										<view class="type">所属公司:{{ item.belongcomp_name }}</view>												
										<view class="type">位置:{{ item.locstr }}</view>
										<view class="delivery-time">购买时间:{{ item.buy_timestr }}</view>
									</view>
									 
								</view>
			
								<view class="bottom">
									<view class="evaluate btn" @click="detail(item.id)">资产详情</view>
								</view>
							</view>
							<u-empty class="epy" text="无资产数据" v-if="!dataList.length"></u-empty>
					
						</view>
					</scroll-view>
	</view>

</template>

<script>
	export default {
		data() {
			return {
				dataList: [
				
				],
				current: 0,
				swiperCurrent: 0,
				tabsHeight: 0,
				dx: 0,
				loadStatus: ['loadmore','loadmore','loadmore','loadmore'],
				list: [],
				title: '',
				backText: '',
				backIconName: 'nav-back',
				right: false,
				showAction: true,
				rightSlot: false,
				useSlot: true,
				scrollHeight:'400px',
				background: {
					'background-image': 'linear-gradient(45deg, rgb(28, 187, 180), rgb(141, 198, 63))'
				},
				isBack: false,
				search: true,
				custom: false,
				isFixed: true,
				keyword: '',
				// #ifdef MP
				slotRight: false,
				// #endif
				// #ifndef MP
				slotRight: false
				// #endif
			}
		},
		computed: {
			slotRightCurrent() {
				return this.slotRight ? 0 : 1;
			}
		},
		methods: {
			renderRecycle(item){
				let data=item.recyclestr;
				if (item.inprocess == "1") {
				        if (item.inprocesstype == "LY") {
				            data = data + "(领用流程中)";
				        } else if (item.inprocesstype == "TK") {
				            data = data + "(退库流程中)";
				        } else if (item.inprocesstype == "JY") {
				            data = data + "(借用流程中)";
				        } else if (item.inprocesstype == "BF") {
				            data = data + "(报废流程中)";
				        } else if (item.inprocesstype == "DB") {
				            data = data + "(调拨流程中)";
				        } else {
				            data = data + "(流程中)";
				        }
				    }
					return data;
			},
			checkclick(obj) {
				if(obj.checked){
					this.selectList.push(obj);
				}else{
					let rind=-1;
					for(let i=0;i<this.selectList.length;i++){
						if(this.selectList[i].id==obj.id){
							rind=i;
							break;
						}
					}
					if(rind>0){
						this.selectList.splice(rind, 1);
					} 
				}
			},
			detail(id){
				this.$u.route({url:"/pages/assets/zcdetail/zcdetail",params: {
					id: id
				}})
			},
			customclick(){
				var dt=this;
				//强制页面重载，跳转到首页
				if( (!this.$u.test.empty(this.keyword))&&this.keyword.length>1  ){
					this.$u.post('/api/base/res/queryResAll.do', {
						search:this.keyword,category:"3"
					}).then(res => {
						if(res.success){
							this.dataList=res.data;
						}else{
						}
					})
				}else{
					uni.showToast({
						title: '请输入关键字',
						icon: "none",
						duration: 1500,
						mask: true
					})
				}
			},
			leftChange(index) {
			},
			searchChange(index) {
			}	 
		}
	}
</script>

<style lang="scss" scoped>
	.epy{
		padding-top:30px;
	}
 
 
	
	.wrap {
		padding: 24rpx;
	}
	
	.navbar-right {
		margin-right: 24rpx;
		display: flex;
	}
	
	.search-wrap {
		margin: 0 20rpx;
		flex: 1;
	}
	
	.right-item {
		margin: 0 12rpx;
		position: relative;
		color: #ffffff;
		display: flex;
	}
	
	.message-box {
		
	}
	
	.slot-wrap {
		display: flex;
		align-items: center;
		flex: 1;
	}
	
	.map-wrap {
		display: flex;
		align-items: center;
		padding: 4px 6px;
		background-color: rgba(240,240, 240, 0.35);
		color: #fff;
		font-size: 22rpx;
		border-radius: 100rpx;
		margin-left: 30rpx;
	}
	
	.map-wrap-text {
		padding: 0 6rpx;
	}
</style>

<style>
/* #ifndef H5 */
page {
	height: 100%;
	background-color: #f2f2f2;
}
/* #endif */
</style>

<style lang="scss" scoped>
.order {
	width: 710rpx;
	background-color: #ffffff;
	margin: 20rpx auto;
	border-radius: 20rpx;
	box-sizing: border-box;
	padding: 20rpx;
	font-size: 28rpx;
	.top {
		display: flex;
		justify-content: space-between;
		.left {
			display: flex;
			align-items: center;
			.store {
				margin: 0 10rpx;
				font-size: 32rpx;
				font-weight: bold;
			}
		}
		.right {
			color: $u-type-warning-dark;
		}
	}
	.item {
		display: flex;
		margin: 20rpx 0 0;
		.left {
			margin-right: 20rpx;
			image {
				width: 200rpx;
				height: 200rpx;
				border-radius: 10rpx;
			}
		}
		.content {
			.title {
				font-size: 28rpx;
				line-height: 50rpx;
			}
			.type {
				margin: 10rpx 0;
				font-size: 24rpx;
				color: $u-tips-color;
			}
			.delivery-time {
				color: #e5d001;
				font-size: 24rpx;
			}
		}
		.right {
			margin-left: 10rpx;
			padding-top: 20rpx;
			text-align: right;
			.decimal {
				font-size: 24rpx;
				margin-top: 4rpx;
			}
			.number {
				color: $u-tips-color;
				font-size: 24rpx;
			}
		}
	}
	.total {
		margin-top: 20rpx;
		text-align: right;
		font-size: 24rpx;
		.total-price {
			font-size: 32rpx;
		}
	}
	.bottom {
		display: flex;
		margin-top: 40rpx;
		padding: 0 10rpx;
		justify-content: space-between;
		align-items: center;
		.btn {
			line-height: 52rpx;
			width: 160rpx;
			border-radius: 26rpx;
			border: 2rpx solid $u-border-color;
			font-size: 26rpx;
			text-align: center;
			color: $u-type-info-dark;
		}
		.evaluate {
			color: $u-type-warning-dark;
			border-color: $u-type-warning-dark;
		}
	}
}
.centre {
	text-align: center;
	margin: 200rpx auto;
	font-size: 32rpx;
	image {
		width: 164rpx;
		height: 164rpx;
		border-radius: 50%;
		margin-bottom: 20rpx;
	}
	.tips {
		font-size: 24rpx;
		color: #999999;
		margin-top: 20rpx;
	}
	.btn {
		margin: 80rpx auto;
		width: 200rpx;
		border-radius: 32rpx;
		line-height: 64rpx;
		color: #ffffff;
		font-size: 26rpx;
		background: linear-gradient(270deg, rgba(249, 116, 90, 1) 0%, rgba(255, 158, 1, 1) 100%);
	}
}
.wrap {
	display: flex;
	flex-direction: column;
	height: calc(100vh - var(--window-top));
	width: 100%;
}
.swiper-box {
	flex: 1;
}
.swiper-item {
	height: 100%;
}
</style>
