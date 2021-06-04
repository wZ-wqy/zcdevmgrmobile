<template>
	<view class="">
		<u-toast ref="uToast"></u-toast>
		<view class="all-card">
			<view class="all-card-top">资产</view>
			<view class="all-card-ct">
				<u-grid :col="col" @click="click" v-if="!isSwiper" :border="border">
					<u-grid-item :index="0" @click="itemClick('add')">
						<u-icon name="shopping-cart" :size="46"></u-icon>
						<view class="grid-text">入库</view>
					</u-grid-item>
					<u-grid-item :index="1" @click="itemClick('discernment')">
						<u-icon name="scan" :size="46"></u-icon>
						<view class="grid-text">识别</view>
					</u-grid-item>
					<u-grid-item :index="2" @click="itemClick('inventory')">
						<u-icon name="calendar" :size="46"></u-icon>
						<view class="grid-text">盘点</view>
					</u-grid-item>
					<u-grid-item :index="3" @click="itemClick('insepection')">
						<u-icon name="search" :size="46"></u-icon>
						<view class="grid-text">巡检</view>
					</u-grid-item>
					<u-grid-item :index="4" @click="itemClick('repair')">
						<u-icon name="car" :size="46"></u-icon>
						<view class="grid-text">报修</view>
					</u-grid-item>
					<u-grid-item :index="5" @click="itemClick('collect')">
						<u-icon name="thumb-up" :size="46"></u-icon>
						<view class="grid-text">领用</view>
					</u-grid-item>
					<u-grid-item :index="6" @click="itemClick('borrow')">
						<u-icon name="bag" :size="46"></u-icon>
						<view class="grid-text">借用</view>
					</u-grid-item>
					<u-grid-item :index="7" @click="itemClick('apply')">
						<u-icon name="grid" :size="46"></u-icon>
						<view class="grid-text">申请</view>
					</u-grid-item>
					<u-grid-item :index="9" @click="itemClick('scrap')">
						<u-icon name="coupon" :size="46"></u-icon>
						<view class="grid-text">报废</view>
					</u-grid-item>
					<u-grid-item :index="10" @click="itemClick('report')">
						<u-icon name="coupon" :size="46"></u-icon>
						<view class="grid-text">调拨</view>
					</u-grid-item>
					<u-grid-item :index="11" @click="itemClick('report')">
						<u-icon name="coupon" :size="46"></u-icon>
						<view class="grid-text">报表</view>
					</u-grid-item>
				</u-grid>

			</view>
		</view>


		<view class="all-card">
			<view class="all-card-top">数据中心</view>
			<view class="all-card-ct">
				<u-grid :col="col" @click="click" v-if="!isSwiper" :border="border">
					<u-grid-item :index="0" @click="itemClick('add')">					 
						<u-icon name="shopping-cart" :size="46"></u-icon>
						<view class="grid-text">机柜</view>
					</u-grid-item>
				 	<u-grid-item :index="1" @click="itemClick('discernment')">
						<u-icon name="scan" :size="46"></u-icon>
						<view class="grid-text">备件</view>
					</u-grid-item>  
				</u-grid>
			</view>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				isSwiper: false,
				border: false,
				col: 3
			};
		},
		methods: {
			click(index) {},
			change(e) {},
			itemClick(type) {
				if (type == "search") {
					this.$u.route({
						url: "/pages/assets/search/search"
					});
				} else if (type == "discernment") {
					this.$u.route({
						url: "/pages/assets/discernment/discernment"
					});
				} else if (type == "insepection") {
					this.$u.route({
						url: "pages/assets/inspection/inspection"
					});
				} else if (type == "inventory") {
					this.$u.route({
						url: "pages/assets/inventory/inventory"
					});
				} else if (type == "repair") {
					this.$u.route({
						url: "pages/assets/repair/repair"
					});
				} else if (type == "scrap") {
					this.$u.route({
						url: "pages/assets/scrap/scrap"
					});
				} else if (type == "report") {
					this.$u.route({
						url: "pages/assets/report/report"
					});
				} else if (type == "collect") {
					this.$u.route({
						url: "pages/assets/collect/collect"
					});
				} else if (type == "borrow") {
					this.$u.route({
						url: "pages/assets/borrow/borrow"
					});
				} else if (type == "add") {
					this.$u.route({
						url: "pages/assets/add/add"
					});
				} else if (type == "apply") {
					this.$u.route({
						url: "pages/assets/apply/apply"
					});
				} else {
					alert("开发中");
				}

			}
		}
	};
</script>

<style scoped lang="scss">
	.grid-text {
		font-size: 28rpx;
		margin-top: 4rpx;
		color: $u-type-info;
	}

	.badge-icon {
		position: absolute;
		width: 40rpx;
		height: 40rpx;
	}

	.swiper {
		height: 480rpx;
	}

	.indicator-dots {
		margin-top: 40rpx;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.indicator-dots-item {
		background-color: $u-tips-color;
		height: 6px;
		width: 6px;
		border-radius: 10px;
		margin: 0 3px;
	}

	.indicator-dots-active {
		background-color: $u-type-primary;
	}
</style>
