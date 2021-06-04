<template>
	<view class="wrap">
		<view>
			<u-toast ref="uToast" />
		</view>
		<view class="u-tabs-box">
			<u-tabs-swiper ref="tabs" :list="tabs" :current="current" @change="change" :is-scroll="false" swiperWidth="750"></u-tabs-swiper>
		</view>
		<swiper class="swiper-box" :current="swiperCurrent" @transition="transition" @animationfinish="animationfinish">
			<swiper-item class="swiper-item">
				<scroll-view scroll-y style="height: 100%;width: 100%;" @scrolltolower="reachBottom">
					<view class="page-box">
						<view>
							<u-empty class="epy" text="无数据" v-if="!list.length"></u-empty>
						</view>
						<view v-for="(item, index) in list" :key="index">
							<u-card :title="item.fuuid" :sub-title="renderStatus(item.fstatus)" sub-title-color="green">
								<view class="" slot="body">
									<view class="content">
										<view class="type">维护人:{{ item.fprocessuser }}</view>
										<view class="type">维护时间:{{ item.fprocesstime }}</view>
										<view class="type">报修原因:{{ item.freason }}</view>
									</view>
								</view>
								<view class="" slot="foot">创建时间:{{item.createTime}}</view>
							</u-card>
						</view>
					</view>
				</scroll-view>
			</swiper-item>
		</swiper>
		<view class="caozuo">
			<u-button @click="openAddPage('/pages/assets/repair/add/add')" style='width:50%' shape="circle" :ripple="true" type="primary">报修</u-button>
		</view>
	</view>
</template>

<script>
	export default {

		data() {
			return {
				disabled: true,
				finishloading: false,
				swiperCurrent: 0,
				tabs: [{
					name: '报修中',
					code: "inprogress"
				}, {
					name: '已完成',
					code: "finish"
				}],
				current: 0,
				list: []
			}
		},
		onLoad() {
			this.loadData(0);
		},
		methods: {
			detail(busid) {

			},
			transition() {},
			animationfinish() {},
			reachBottom() {

			},

			finish(busid) {
				this.finishloading = true;
				this.$u.post('/api/zc/resInspection/ext/finishInspect.do', {
					busid: busid
				}).then(res => {
					this.finishloading = false;
					let type = "error"
					if (res.success) {
						type = "success";
						this.loadData(this.current)
					}
					this.$refs.uToast.show({
						title: res.message,
						position: "center",
						type: type,
						icon: true,
						url: ''
					});
				})
			},
			openAddPage(path) {
				this.$u.route({
					url: path
				});
			},
			renderMethod(value) {
				if (value == "free") {
					return "自由模式"
				} else if (value == "fix") {
					return "固定模式"
				} else {
					return value;
				}
			},
			renderName(item) {
				let v = item.name;
				if (item.status == 'finish') {
					v = v + "--已完成";
				} else if (item.status == 'wait') {
					v = v + "--待巡检";
				} else if (item.status == 'acting') {
					v = v + "--巡检中";
				} else {
					v = v + "--" + item.status;
				}
				return v;
			},
			renderStatus(status) {
				if (status == "underrepair") {
					return "报修中";
				} else if (status == "cancel") {
					return "已取消";
				} else if (status == "finish") {
					return "已完成";
				} else {
					return status;
				}
			},
			loadData: function(index) {
				this.$u.post('/api/zc/resRepair/ext/selectMyList.do', {
					statustype: this.tabs[index].code
				}).then(res => {
					if (res.success) {
						this.list = res.data;
					}
				})
			},
			change(index) {
				this.current = index;
				this.loadData(index);
			}
		}
	}
</script>

<style lang="scss" scoped>
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

	}

	.renderMethod {
		margin-left: 3rpx;
		margin-top: 8rpx;
		color: $u-type-primary;
		margin-bottom: 30rpx;
	}

	.btn {
		margin-bottom: 25rpx;
		margin-top: 45rpx;
	}
</style>
